# PicoPhone

A DIY mobile phone built around a Raspberry Pi Pico 2 (RP2350) and a SIM800C GSM modem, with a 3.5" touchscreen, a LiPo battery, and a USB-C charger.

This is primarily a personal build log. Forks and your own builds are welcome.

> **Status:** work in progress. Hardware and firmware are under active development and have not been fully bench-verified yet. Expect rough edges.

## Features (v1 scope)

- Voice calls, with call log
- SMS: single and multi-part messages, GSM-7 and UCS-2 (PDU mode)
- Contacts
- Lockscreen, notification center, settings, alarms, on-screen QWERTY keyboard
- Network time sync (NITZ) with manual override
- Low-power sleep with wake on call, SMS, touch, or power button

Out of scope for v1: Wi-Fi/Bluetooth, mobile data, camera, GPS, USSD, SIM PIN entry, OTA updates.

## Hardware

| Part | Role |
|---|---|
| Raspberry Pi Pico 2 (RP2350) | Main MCU |
| SIM800C breakout (14-pin) | GSM voice and SMS |
| ZJY-TFT350-11P-TOUCH | 320x480 ILI9486L display with XPT2046 resistive touch, shared SPI bus |
| BQ25895 | USB-C charging, power path, battery voltage ADC |
| TS3A5018 | Audio path switch (earpiece / headset) |
| 3.5 mm TRRS jack | Headset |
| 4-layer custom PCB | Carrier board |

Several details of the display module are still marked as unverified in my notes (color depth, pin order, backlight current), so treat the wiring as provisional until confirmed on the bench.

## Firmware

- C99 on the Pico SDK 2.2.0 (`pico2`, `rp2350-arm-s`), built with CMake
- No RTOS: a single-core cooperative loop
- LVGL v9.2.2 for the UI
- LittleFS and a key-value store on the onboard flash
- Dependencies are pinned in `lib/versions.lock`

Build:

```bash
git clone --recurse-submodules <this repository>
cd picophone
cmake -B build -S .
cmake --build build
```

Then hold BOOTSEL on the Pico, plug it in over USB, and copy `build/pico_phone.uf2` onto the drive that appears.

## Documentation

Design notes (architecture, netlist, BOM, PCB design) live in `docs/`.

## License

This project (firmware, hardware design files, and documentation) is licensed under the GNU General Public License v3.0 or later. See `LICENSE`.

If you build on this work and distribute it, the GPL requires you to share your changes under the same license. Either way, I'd love to hear about your build.

Third-party components (Pico SDK, LVGL, LittleFS, and others) keep their own licenses.

## Safety

This project involves LiPo batteries, high-current radio transmission, and a device that connects to the public cellular network. Build and use it at your own risk, and make sure it complies with the rules where you live.

## Acknowledgements

Built with the help of [Claude](https://claude.ai), an AI assistant made by Anthropic.
