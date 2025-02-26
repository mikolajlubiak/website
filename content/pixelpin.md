---
title: "PixelPin"
date: 2024-02-18T15:00:00+01:00
tags: ['arduino', 'embedded', 'esp32', 'c++', 'c', 'bluetooth', 'app', 'flutter']
---

# Background

Someday I thought that it would be cool to have a pin or a patch on my backpack. Sadly, I didn’t have any… Sooo the next logical step was to make one using E Ink.

# Hardware

For hardware, I chose… whatever I had lying around. That just happened to be an ESP32S3 (way overpowered, later changed for an ESP32C3) and an E Ink display.

# [Code, the microcontroller](https://github.com/mikolajlubiak/pixelpin)

I decided to choose Bluetooth (BLE) for the communication protocol, since it’s supposed to use little energy. Once I had a simple communication channel similar to serial Bluetooth working, I was pretty happy with my results. I could `BEGIN` the communication, say that I wanted to use `PNG` or `JPEG`, send the binary data of the image, `END` the communication and request the image to be `DRAW`n. The image format that the library expects is  also quite interesting. It wanted to get two buffers—mono and color—with 1 bit per pixel.
```cpp
whitish = (red * 0.299f + green * 0.587f + blue * 0.114f) > 0x80;
colored = ((red > 0x80) && (((red > green + 0x40) && (red > blue + 0x40))
            || (red + 0x10 > green + blue))) || (green > 0xC8 && red > 0xC8 && blue < 0x40);
if (whitish) { } 
else if (colored) { out_color_byte &= ~(0x80 >> col % 8); }
else { out_byte &= ~(0x80 >> col % 8); }
```

![The prototype on a white background](/pixelpin1.jpg "The prototype on a white background")

# [Code, the app](https://github.com/mikolajlubiak/pixelpin-app)

And all that was sent using an external serial Bluetooth terminal app. But I thought to myself, Why not make a custom app for it? Then I could offload the image decoding from the little MCU to the app that’s running on much better hardware. I decided to choose Flutter for the app since I had some experience with it. I actually tried to make a Bluetooth-data-sending app before, but I failed miserably. To my surprise, I have actually learned something throughout this time, since I did succeed at making the app and the MCU talk through Bluetooth.

# The prototype

Once I had basically everything working, I texted my friend that I needed his help with making da embedded project actually embedded. Because up to that point, it had been constantly hooked up to my computer. I wanted him to make me a case and add some electrical current source there. We had some issues, but even more hot glue. The measurements were made for a different battery then ended up in the prototype, and it was a little too thick. Hopefully it was no match for the hot glue gun, and we were happily holding the worki… Ooo shoot, it couldn’t just work, right? After some debugging, we found out that the FPC connector of the display had disconnected. After fixing that, we actually had the working prototype in our hands.

![The prototype in my hand](/pixelpin2.jpg "The prototype in my hand")

