![GitHub Release](https://img.shields.io/github/v/release/sfDuhNinja/WPalaControl)
![Home Assistant](https://img.shields.io/badge/home_assistant-2024.11-blue.svg?logo=homeassistant)

# WPalaControl (Ferroli / Fumis Alpha fork)

> Full credit for **WPalaControl** (hardware, firmware base, Fumis/Palazzetti protocol) goes to **[Domochip](https://github.com/Domochip/WPalaControl)**. This fork is unaffiliated and unendorsed — it just carries a redesigned web dashboard (dark mode, live status widget, reorganized accordion UI, mobile fixes) for my own Ferroli/Fumis Alpha setup. Firmware updates point to this repo instead of upstream.
>
> Everything else — full HTTP/MQTT API, stove compatibility, Home Assistant/Jeedom integration, hardware schematics — is documented upstream: **[Domochip/WPalaControl](https://github.com/Domochip/WPalaControl)**.

## Get the adapter

- 🔧 Build it yourself: [BUILD.md](https://github.com/Domochip/WPalaControl/blob/master/BUILD.md) (KiCad + 3D-printable enclosure)
- 🛍️ Buy one: [WPalaControl on Lectronz](https://lectronz.com/products/wpalacontrol)
- ⭐ Original project: [Domochip/WPalaControl](https://github.com/Domochip/WPalaControl)


## What's different in this fork

Only the web UI (`src/base/data/`, `src/data/`) and the OTA source pointer (`src/Main.h`) were touched — firmware core, protocol handling, MQTT, and HA discovery are untouched.

- Redesigned UI: dark mode, live "hero" status widget, exclusive accordion sections, reorganized Status page
- Fixed: serialized stove data requests on Status load (was up to 9 concurrent, unsafe on ESP8266), stuck mobile reboot menu, firmware upload timeout (30s → 120s), auto update-check on load, clearer Wi-Fi status text
- Removed: hardware/build files, old screenshots, unused CI/test scaffolding — see upstream for those

Personal fork, tested only on my own setup — not vetted to upstream contribution standards.
