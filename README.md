![GitHub Release](https://img.shields.io/github/v/release/sfDuhNinja/FerControl)
![Home Assistant](https://img.shields.io/badge/home_assistant-2024.11-blue.svg?logo=homeassistant)

# FerControl (Ferroli / Fumis Alpha fork of WPalaControl)

> Full credit for the original **WPalaControl** project (hardware, firmware base, Fumis/Palazzetti protocol) goes to **[Domochip](https://github.com/Domochip/WPalaControl)**. This fork is unaffiliated and unendorsed; renamed to **FerControl** to keep it clearly distinct from upstream. Firmware updates (OTA + web upload) point to this repo instead of upstream.
>
> Full HTTP/MQTT API, stove compatibility, and hardware schematics are documented upstream: **[Domochip/WPalaControl](https://github.com/Domochip/WPalaControl)**.

## Get the adapter

- 🔧 Build it yourself: [BUILD.md](https://github.com/Domochip/WPalaControl/blob/master/BUILD.md) (KiCad + 3D-printable enclosure)
- 🛍️ Buy one: [WPalaControl on Lectronz](https://lectronz.com/products/wpalacontrol)
- ⭐ Original project: [Domochip/WPalaControl](https://github.com/Domochip/WPalaControl)

## What's different in this fork

- Redesigned web UI (dark mode, live status widget, accordion layout) tailored to my Ferroli BioPellet Tech 30 (buffer tank configuration)
- Ongoing firmware stability and correctness fixes on top of upstream's base
- Removed: hardware/build files, old screenshots, unused CI/test scaffolding (see upstream for those)

Personal fork, tested only on my own setup, not vetted to upstream contribution standards. Written with the help of [Claude](https://claude.com).
