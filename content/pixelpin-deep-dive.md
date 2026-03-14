---
title: "BLE Protocol Design for Embedded Wearables: The PixelPin Deep Dive"
date: 2026-03-14
tags: ['embedded', 'esp32', 'c++', 'c', 'bluetooth', 'ble', 'e-paper', 'protocol-design', 'app', 'flutter']
---

# BLE Protocol Design for Embedded Wearables: The PixelPin Deep Dive

[PixelPin](https://github.com/mikolajlubiak/pixelpin) is a wearable e-paper display pin I designed and built. It's powered by an ESP32, receives images over Bluetooth Low Energy from a [Flutter companion app](https://github.com/mikolajlubiak/pixelpin-app), applies Floyd-Steinberg dithering for 3-color output, and renders the result on an e-paper display.

This post covers the hardest parts: the custom BLE protocol, the image processing pipeline, and the bit-level manipulations needed to drive an e-paper display.

## The Hardware

```
┌─────────────┐     BLE      ┌─────────────┐     SPI     ┌─────────────┐
│   Phone     │─────────────▶│   ESP32     │────────────▶│  E-Paper    │
│   (Flutter  │  Custom      │             │             │  Display    │
│    App)     │  Protocol    │  Image      │  Raw bits   │  (3-color)  │
│             │              │  Processing │             │             │
└─────────────┘              └─────────────┘             └─────────────┘
```

The e-paper display I used supports three colors: **black**, **white**, and **red**. Each pixel requires 2 bits of information (one bit in the black/white buffer, one bit in the red buffer). The display resolution determines how much data needs to be transferred over BLE.

## The Problem: BLE is Slow

BLE's Maximum Transmission Unit (MTU) is typically **20 bytes** by default (can be negotiated up to ~512 bytes). An image for the e-paper display could be several kilobytes. That means:

1. We can't send the image in one packet
2. We need a way to chunk the image, send it reliably, and reassemble it
3. We need commands beyond just "here's image data" (clear display, set parameters, etc.)

This is why I needed a **custom binary protocol**.

## The Protocol

### State Machine

The ESP32 firmware runs a state machine:

```
┌─────────┐  CONNECT   ┌──────────┐  CMD_START   ┌───────────┐
│  IDLE   │───────────▶│ CONNECTED│─────────────▶│ RECEIVING │
│         │            │          │               │  IMAGE    │
└─────────┘            └──────────┘               ├───────────┤
                            ▲                     │ Accumulate│
                            │                     │ chunks    │
                            │         CMD_END     │           │
                            │    ┌────────────────┤           │
                            │    ▼                └───────────┘
                       ┌──────────┐
                       │PROCESSING│
                       │          │──▶ Dither ──▶ Display ──▶ CONNECTED
                       └──────────┘
```

### Packet Structure

Every packet starts with a 1-byte command identifier:

```
┌──────────┬──────────────────────────────┐
│ CMD (1B) │ PAYLOAD (0 to MTU-1 bytes)   │
└──────────┴──────────────────────────────┘
```

Command types:
| CMD | Name | Payload | Description |
|-----|------|---------|-------------|
| 0x01 | START | width (2B) + height (2B) | Begin image transfer, allocate buffer |
| 0x02 | DATA | raw pixel bytes | Image data chunk |
| 0x03 | END | none | Transfer complete, begin processing |
| 0x04 | CLEAR | none | Clear the display |
| 0x05 | STATUS | none | Request device status |

### Chunked Transfer

For a DATA command, the payload is a raw chunk of the image. The ESP32 keeps track of how many bytes it has received and where to write the next chunk:

```cpp
case CMD_DATA:
    memcpy(image_buffer + bytes_received, payload, payload_length);
    bytes_received += payload_length;
    // No ACK needed — BLE GATT handles reliability at the link layer
    break;
```

The app sends chunks as fast as BLE allows. We don't implement our own acknowledgment because BLE's GATT write-with-response already guarantees delivery at the link layer.

## Image Processing: Floyd-Steinberg Dithering

The phone sends a full-color image. The e-paper display can only show 3 colors. We need to convert arbitrary RGB pixels into black, white, or red — and make it look good.

**Floyd-Steinberg dithering** distributes the quantization error of each pixel to its neighbors:

```
           current pixel
              [*]  7/16 →
        3/16   5/16   1/16
          ↙     ↓      ↘
```

For each pixel:
1. Find the closest available color (black, white, or red)
2. Calculate the error (difference between original and chosen color)
3. Distribute that error to neighboring pixels using the weights above

```cpp
for (int y = 0; y < height; y++) {
    for (int x = 0; x < width; x++) {
        Color old_pixel = image[y][x];
        Color new_pixel = closest_color(old_pixel);  // black, white, or red
        image[y][x] = new_pixel;

        Color error = old_pixel - new_pixel;

        if (x + 1 < width)
            image[y][x+1]     += error * 7.0/16.0;
        if (y + 1 < height) {
            if (x > 0)
                image[y+1][x-1] += error * 3.0/16.0;
            image[y+1][x]       += error * 5.0/16.0;
            if (x + 1 < width)
                image[y+1][x+1] += error * 1.0/16.0;
        }
    }
}
```

The result: smooth gradients are approximated by patterns of dots, creating the illusion of more colors than the display actually supports.

## Bit Manipulation: Packing Pixels for E-Paper

E-paper displays expect a very specific bit format. For a 3-color display, there are typically two separate buffers:

1. **Black/White buffer**: 1 bit per pixel (0 = black, 1 = white)
2. **Red buffer**: 1 bit per pixel (1 = red, overrides B/W)

8 pixels are packed into 1 byte, MSB first:

```cpp
void pack_pixel(uint8_t* bw_buffer, uint8_t* red_buffer,
                int x, int y, int width, Color pixel) {
    int byte_index = (y * width + x) / 8;
    int bit_index  = 7 - ((y * width + x) % 8);  // MSB first

    if (pixel == BLACK) {
        bw_buffer[byte_index]  &= ~(1 << bit_index);  // clear = black
        red_buffer[byte_index] &= ~(1 << bit_index);
    } else if (pixel == WHITE) {
        bw_buffer[byte_index]  |=  (1 << bit_index);  // set = white
        red_buffer[byte_index] &= ~(1 << bit_index);
    } else { // RED
        bw_buffer[byte_index]  &= ~(1 << bit_index);
        red_buffer[byte_index] |=  (1 << bit_index);   // set = red
    }
}
```

This kind of bit masking and shifting is where embedded programming gets fun (or painful, depending on your perspective). Get one bit wrong and you get corrupted images.

## Performance on ESP32

The ESP32 has limited RAM (~320KB usable). For a 200x200 pixel display:
- Raw image (RGB): 200 × 200 × 3 = 120,000 bytes = 117KB
- B/W buffer: 200 × 200 / 8 = 5,000 bytes
- Red buffer: 200 × 200 / 8 = 5,000 bytes

The dithering is done in-place to minimize memory usage. The BLE transfer of 120KB at ~20 bytes/packet takes about 10-15 seconds — which is acceptable for a display that only updates when you want to change the image.

## What I Learned

1. **Design your protocol before writing code.** I initially tried ad-hoc packet handling and it became unmaintainable. The state machine + command byte design made everything cleaner.

2. **BLE is reliable enough at the GATT layer.** You don't need application-level ACKs for write-with-response. But you do need them for write-without-response if you choose that path for speed.

3. **Dithering quality depends on color distance metrics.** Using Euclidean distance in RGB space gives different (often worse) results than perceptual color spaces like CIELAB. I used RGB for simplicity on the ESP32.

4. **E-paper refresh is slow.** The display takes 2-3 seconds to refresh. This isn't a bug — it's physics (electrophoretic ink particles moving). Design your UX around it.

5. **Bit manipulation bugs are invisible until they're not.** Off-by-one errors in bit indexing produce images that look *almost* right but with subtle column shifts or color inversions. Systematic testing with known patterns (checkerboard, solid colors) catches these.

---

*Source code: [ESP32 firmware](https://github.com/mikolajlubiak/pixelpin), [Flutter app](https://github.com/mikolajlubiak/pixelpin-app). See also my posts on [CPU rendering on ESP32](/blog/vulkan-six-iterations) and [the Intel cache bug](/blog/intel-cache-thrashing-bug).*
