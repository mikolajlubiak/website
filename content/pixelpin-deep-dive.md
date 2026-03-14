---
title: "BLE Protocol Design for Embedded Wearables: The PixelPin Deep Dive"
date: 2026-03-14
tags: ['embedded', 'esp32', 'c++', 'c', 'bluetooth', 'ble', 'e-paper', 'protocol-design', 'app', 'flutter']
---

[PixelPin](https://github.com/mikolajlubiak/pixelpin) is an ESP32-powered e-paper wearable that leverages a Flutter companion app for image pre-processing. By shifting heavy computational tasks - specifically resizing and color space conversion - to the smartphone, the firmware is optimized for high-speed data ingestion and specialized dithering for 3-color electrophoretic displays.

This post breaks down the high-throughput BLE implementation, the RGB565-to-EPD pipeline, and the bit-packing logic required for the hardware.

## Hardware Architecture

The system utilizes a 4-wire SPI interface to drive a 3-color (Black, White, Red) display.

```
┌─────────────┐    BLE (GATT)    ┌─────────────┐     SPI     ┌─────────────┐
│   Phone     │─────────────────▶│   ESP32     │────────────▶│  E-Paper    │
│ (RGB565 Pre-│  512B MTU        │ (Dithering  │  Raw Bits   │  Display    │
│ processing) │  High Speed      │ & Packing)  │ (2 Buffers) │  (3-color)  │
└─────────────┘                  └─────────────┘             └─────────────┘
```

## High-Throughput BLE Implementation

Performance bottlenecks in BLE often stem from default MTU (Maximum Transmission Unit) constraints of 20 bytes. PixelPin negotiates a **512-byte MTU** to maximize throughput.

By utilizing large payloads and the `WRITE_WITHOUT_RESPONSE` characteristic, the image transfer is effectively instant. For a 200x200 display using 16-bit color, the 80KB payload is delivered in approximately 160 packets, saturating the BLE bandwidth and minimizing radio uptime.

### The Data Format: RGB565
To optimize memory alignment and reduce CPU cycles on the ESP32, the app streams the image in **RGB565** format (2 bytes per pixel). This avoids the overhead of decoding JPEGs or bit-shifting standard 24-bit RGB on the MCU.

## The Custom Binary Protocol

The firmware implements a synchronous state machine to handle the incoming stream.

### Packet Structure
Every packet starts with a 1-byte Command Identifier (CID):

```
┌──────────┬─────────────────────────────────┐
│ CID (1B) │ PAYLOAD (Up to 511 bytes)       │
└──────────┴─────────────────────────────────┘
```

| CID | Command | Payload | Description |
|-----|---------|---------|-------------|
| 0x01 | START   | `uint16_t` W, H | Allocates image buffer based on dimensions. |
| 0x02 | DATA    | `uint8_t[]` | Raw RGB565 chunks. |
| 0x03 | END     | None | Signals transfer completion and triggers processing. |
| 0x04 | CLEAR   | None | Flushes the display buffers. |

## Image Processing: Floyd-Steinberg Dithering

The EPD is a 3-color device, but the input is 16-bit color. We use **Floyd-Steinberg error diffusion** to map the RGB565 space into the physical palette (Black, White, Red) while preserving visual gradients.

The algorithm processes the `image_buffer` in-place to save SRAM. For each pixel, it calculates the quantization error - the difference between the RGB565 value and the closest hardware color - and distributes that error to neighboring pixels.

```cpp
// RGB565 decomposition
uint16_t pixel = image_buffer[i];
int r = (pixel >> 11) & 0x1F;
int g = (pixel >> 5) & 0x3F;
int b = pixel & 0x1F;

// Find closest match in {Black, White, Red}
Color closest = find_nearest_neighbor(r, g, b);
Error err = original - closest;

// Error distribution (7/16, 3/16, 5/16, 1/16)
apply_error(x + 1, y,     err * 7/16);
apply_error(x - 1, y + 1, err * 3/16);
apply_error(x,     y + 1, err * 5/16);
apply_error(x + 1, y + 1, err * 1/16);
```

## Bit Manipulation: Dual-Buffer Packing

E-paper controllers require two distinct bit-planes, where each bit represents a pixel (8 pixels per byte, MSB first):

1.  **B/W Buffer**: `0` = Black, `1` = White.
2.  **Red Buffer**: `1` = Red (overrides B/W), `0` = No Red.

```cpp
void pack_pixel(int x, int y, Color color) {
    int byte_idx = (y * width + x) / 8;
    uint8_t bit = 1 << (7 - (x % 8)); // MSB first

    if (color == RED) {
        red_buffer[byte_idx] |= bit;
    } else {
        red_buffer[byte_idx] &= ~bit;
        if (color == WHITE) bw_buffer[byte_idx] |= bit;
        else bw_buffer[byte_idx] &= ~bit; // BLACK
    }
}
```

## Performance & Lessons Learned

* **BLE Throughput**: With a 512B MTU, the 80KB transfer is nearly instantaneous. The real bottleneck remains the physical refresh rate of the electrophoretic ink (typically 2–15 seconds).
* **Offload to the App**: Pre-converting to RGB565 on the phone saved significant CPU cycles and code complexity on the ESP32.
* **Memory Management**: With ~320KB SRAM available, the ESP32 comfortably holds the 80KB raw buffer and the required bit-buffers for a 200x200 display.
* **Protocol Design**: A state-machine-driven protocol with a clear CID prefix is far more robust than attempting to parse ad-hoc streams.

*Source code: [ESP32 firmware](https://github.com/mikolajlubiak/pixelpin), [Flutter app](https://github.com/mikolajlubiak/pixelpin-app).*
