![GitHub Release](https://img.shields.io/github/v/release/sfDuhNinja/WPalaControl)
![Home Assistant](https://img.shields.io/badge/home_assistant-2024.11-blue.svg?logo=homeassistant)

# WPalaControl (Ferroli / Fumis Alpha fork)

> All credit for **WPalaControl** — the adapter hardware design, the firmware base, and the Fumis/Palazzetti protocol implementation — belongs entirely to **[Domochip](https://github.com/Domochip)** and the [original WPalaControl project](https://github.com/Domochip/WPalaControl). This fork is not affiliated with or endorsed by Domochip.
>
> This fork is maintained for my own setup: a **Ferroli stove using the Fumis Alpha controller**. It carries a redesigned embedded web dashboard (new visual style, dark mode, a live status "hero" widget, reorganized Status/Config/Firmware sections, plus a handful of usability/mobile fixes) — see ["What changed in this redesign"](#what-changed-in-this-redesign) below. Firmware update checks and releases point to this repo (`sfDuhNinja/WPalaControl`), not upstream. If you want the original, actively maintained project supporting adapter sales, schematics, and enclosure files, use [Domochip/WPalaControl](https://github.com/Domochip/WPalaControl) directly.

**WPalaControl** is a device designed to control and monitor Fumis-based stoves using a D1 Mini.  
It allows you to power on/off, adjust the set point, change fan speeds, and retrieve status, alarms, and temperature data from your stove.  

It supports native HTTP requests which makes it fully compatible with all existing Palazzetti/Fumis home automation plugins/apps.  

It also integrates with MQTT to monitor/control your stove in a much more efficient way.  

## Stove Compatibility

The underlying protocol (unchanged from upstream) works with any stove using the Fumis Controller, used by many manufacturers, including:  
  
Palazzetti / Jotul / TurboFonte / Godin / Fonte Flamme / Invicta / Casatelli / Alpis / Faizen / HETA / Ferroli / ...

This fork is specifically built and tested against a **Ferroli** stove with the **Fumis Alpha** controller — other brands should keep working (same protocol), but are untested here.

To determine if your stove is compatible with WPalaControl, look for the following Control Panel or Motherboard:  
![Fumis Controller](img/fumis2.png)![Fumis Controller](img/fumis.png)

## Home Automation compatibility

| <img src="https://www.home-assistant.io/images/home-assistant-logo-vertical.svg" alt="Logo HASS" height="128"> | [MQTT Integration](https://www.home-assistant.io/integrations/mqtt/) (autodiscovery) <br> Most options and fans control | ![hacard](img/hacard.png) |
| ------------- | ------------- | ------------- |
| <img src="https://www.home-assistant.io/images/home-assistant-logo-vertical.svg" alt="Logo HASS" height="128"> | [Palazzetti Integration](https://www.home-assistant.io/integrations/palazzetti)<br> (2024.11 minimum) <br> Basic implementation for now | ![hacard-pala-integ](img/hacard-pala-integ.png) |
| ![Logo Jeedom](https://www.jeedom.com/_next/image?url=%2Fassets%2Fimg%2Flogo.png&w=256&q=75) | [Palazzetti plugin](https://www.jeedom.com/market/index.php?v=d&p=market_display&id=3104) | ![Jeedom widget](https://domotechdiscord.github.io/Docs/Palazzetti/images/widget_1.png) |

## Get the adapter

![complete Module](./img/complete-module.jpg)

This fork doesn't host adapter schematics, enclosure files, or sales info — full credit and support goes to Domochip, who designed the adapter:

- 🔧 Build it yourself: [BUILD.md on Domochip/WPalaControl](https://github.com/Domochip/WPalaControl/blob/master/BUILD.md) (KiCad schematics + 3D-printable enclosure)
- 🛍️ Buy one ready-made from Domochip: [WPalaControl on Lectronz](https://lectronz.com/products/wpalacontrol)
- ⭐ Or check out the original project: [Domochip/WPalaControl](https://github.com/Domochip/WPalaControl)

## Connect

⚠️ **Before connecting, please check that you are using the correct cable**  
For Hardware V1.x: please use a **Crossed** cable  
For Hardware V2.x: please use a **Straight** cable  
_(Hardware version is written on the box and on the PCB)_

![WPalaControl rj11](img/rj11-pinout.png)

✅If your stove has an accessible RJ12 connector, simply connect the cable.

## First Boot

During the first Boot, the WPalaControl creates its own Wi-Fi network (AP mode)  

Firmware version **3.2.0 and higher**:  

- Network SSID : `WPalaControl`
- Password : `password`

Version **3.1.x and lower**:  

- Network SSID : `WirelessPalaControlXXXX`
- Password : `PasswordPalaControl`

You should be automatically redirected to the module page.  
If not, go to 👉 <http://wpalacontrol.local> 👈

## Configuration pages

> 📸 The screenshots below are from the original Domochip UI and don't yet reflect this fork's redesign (new visual style, dark mode, live hero widget, reorganized sections). Same functionality, different look — see [below](#what-changed-in-this-redesign) for details.

### Status

Provides useful informations about the module and the stove.  
**The stove SN appears then followed, 1 minute later, by refreshed stove information**  
<img src="img/status.png" alt="status screenshot" width="400" style="vertical-align: top;"><img src="img/status2.png" alt="status2 screenshot"  width="400">

### Config

![config screenshot](img/config.png)

#### System

- **Reboot**: restart the module
- **Reboot in Rescue Mode**: restart the module with default configuration

#### WiFi

- **ssid & password**: IDs of your Wifi Network  
- **hostname**: name of ESP on the network  
- **IP, GW, NetMask, DNS1&2**: Fixed IP configuration  

![configMQTT screenshot](img/configMQTT.png)

#### PalaControl - Home Automation

- **Type**: enable/disable MQTT communication  
- **Upload Period**: set the refresh interval of stove information (in seconds)  
- **Hostname, Port, Username, Password**: MQTT server details (username and password are - optional)  
- **Base Topic**: prefix for WPalaControl topic structure  
- **MQTT Type**: defines MQTT topics and data structure:  
  - **Generic**: publish raw values under the base topic (e.g. "{baseTopic}/T1" = "*20.00*")
  - **Generic JSON**: publish values JSON to "category" topic under base topic (e.g. "{baseTopic}/TMPS" = "*{"INFO":{"CMD":"GET TMPS","RSP":"OK......*")
  - **Generic Categorized**: publish raw values to "category" topic under base topic (e.g. "{baseTopic}/TMPS/T1" = "*20.00*")
- **Home Assistant Discovery**: Enable/disable Home Assistant auto-discovery

### Firmware

Aallows you to flash a new firmware version directly from GitHub releases or using `WPalaControl.*.bin` or `WirelessPalaControl.*.bin` file:  
![firmware screenshot](img/firmware.png)  
Ensure the module has Internet access and/or DNS configured if IP address is static

## Use it

### HTTP

Natively, HTTP GET request can be sent directly to the module.  

```
http://wpalacontrol.local/cgi-bin/sendmsg.lua?cmd={command}
```

### MQTT

Send commands via MQTT to `%BaseTopic%/cmd` topic once MQTT is configured.  
Execution result is:

- published following the configured MQTT Type
- published on `%BaseTopic%/result` in JSON format

Module connection status is published to `%BaseTopic%/connected`:

- 0: not connected
- 1: connected but no communication with the stove
- 2: module and stove connected

### Command List
  
- `GET+STDT`: get static data
- `GET+ALLS`: get all status data
- `GET+STAT`: get status of the stove⏲️
- `GET+TMPS`: get temperatures of the stove⏲️
- `GET+FAND`: get Fan values⏲️
- `GET+SETP`: get current Set Point (desired temperature)⏲️
- `GET+POWR`: get current power (1-5)⏲️
- `GET+CUNT`: get some counters
- `GET+CNTR`: get some counters (same as GET+CUNT)⏲️
- `GET+DPRS`: get delta pressure data⏲️
- `GET+TIME`: get stove clock data⏲️
- `GET+IOPT`: get IO ports status
- `GET+SERN`: get stove Serial Number
- `GET+MDVE`: get stove model and fw version
- `GET+CHRD`: get chrono data
- `GET+PARM+92`: get parameter (ex : 92=pellet type (1-3))
- `GET+HPAR+57`: get hidden parameter (ex : 57=% of pellet to feed for pellet type 3)
- `BKP+PARM+CSV`: get all parameters in a CSV file (HTTP only) ✨
- `BKP+PARM+JSON`: get all parameters in a JSON file (HTTP only) ✨
- `BKP+HPAR+CSV`: get all hidden parameters in a CSV file (HTTP only) ✨
- `BKP+HPAR+JSON`: get all hidden parameters in a JSON file (HTTP only) ✨
- `CMD+ON`: turn stove ON
- `CMD+OFF`: turn stove OFF
- `SET+POWR+3`: set power (1-5)
- `SET+PWRU`: increase power by 1 unit
- `SET+PWRD`: decrease power by 1 unit
- `SET+SETP+20`: set Set Point (desired temperature)
- `SET+STPF+19.8`: set Set Point with a 0.2° precision (depend of your stove model)✨
- `SET+STPU`: increase Set Point by 1 unit
- `SET+STPD`: decrease Set Point by 1 unit
- `SET+RFAN+7`: set Room Fan value (0-5;6=Max;7=Auto)
- `SET+FN2U`: increase Room Fan by 1 unit
- `SET+FN2D`: decrease Room Fan by 1 unit
- `SET+FN3L+0`: set Room Fan 3 value (0-5)
- `SET+FN4L+0`: set Room Fan 4 value (0-5)
- `SET+SLNT+0`: set Silent mode value (0-1)
- `SET+TIME+2023-12-28+19:42:00`: set stove Date and Time (2000-2099) (1-12) (1-31) (0-23) (0-59) (0-59)
- `SET+CSST+0`: set Chrono Status value (0-1)
- `SET+CSTH+2+18`: set Chrono Program Start Hour (1-6) (0-23)
- `SET+CSTM+2+30`: set Chrono Program Start Minute (1-6) (0-59)
- `SET+CSPH+2+22`: set Chrono Program Stop Hour (1-6) (0-23)
- `SET+CSPM+2+45`: set Chrono Program Stop Minute (1-6) (0-59)
- `SET+CSET+2+19`: set Chrono Program Set Point (1-6) (desired temperature)
- `SET+CDAY+7+3+6`: set Chrono Program for week day (Day-Mem-Prog) (1-7) (1-3) (1-6)
- `SET+CPRD+1+19+18+30+22+45`: set Chrono Program data (Prog-Temp-StartH-StartM-StopH-StopM) (1-6) (temperature) (0-23) (0-59) (0-23) (0-59)
- `SET+PARM+92+2`: set parameter 92 to value 2 (ex : 92=pellet type (1-3))
- `SET+HPAR+57+95`: set hidden parameter 57 to value 95 (ex : 57=% of pellet to feed for pellet type 3)

⏲️: Published automatically  
✨: WPalaControl specific commands

### Description

MQTT infos published every "Upload Period":

- `STATUS`, `LSTATUS`, `FSTATUS`: status of the stove
- `T1`, `T2`, `T3`, `T4`, `T5`: temperature of the stove
- `F1V`, `F2V`, `F2L`, `F2LF`, `F3L`, `F4L`: fan values (meaning depend of your stove model)
- `IGN`: ignition counter
- `IGNERRORS`: ignition error counter
- `POWERTIME`: total heating time (hour:minute)
- `HEATTIME`: ??? (hour:minute)
- `SERVICETIME`: heating time since last maintenance (hour:minute)
- `ONTIME`: time from last power ON (hour:minute)
- `OVERTMPERRORS`: overtemperature error counter
- `PQT`: wood pellet consumption
- `STOVE_DATETIME`: date of the stove
- `STOVE_WDAY`: week day of the stove
- `SETP`: current Set Point (desired temperature)
- `SECO`: ECO Mode Restart Temperature Threshold (PAR50: 6 -> 0.6°C)
- `BECO`: ECO Mode Active
- `PWR`: current power (1-5)
- `FDR`: feeder
- `DP_TARGET`: delta pressure target
- `DP_PRESS`: actual delta pressure

## What changed in this redesign

This fork only touches the embedded web UI (`src/base/data/`, `src/data/`) and the OTA/release source (`CUSTOM_APP_MANUFACTURER` in `src/Main.h`, pointing update checks at this repo instead of upstream). Nothing in the firmware's core logic, the Fumis/Palazzetti protocol handling, MQTT, or Home Assistant discovery structure was modified.

**Visual redesign:**
- New visual style (self-hosted fonts, custom color system, dark mode via `prefers-color-scheme`)
- Status page reworked around a "hero" widget showing live stove values, with configurable slots
- Status/Config/Firmware turned into an accordion, exclusive (opening one section closes the others)
- Status page reorganized into clearly separated Module / Wi-Fi / Stove blocks

**Fixes found while redesigning:**
- Serialized the stove data requests fired when opening Status (previously up to 9 concurrent requests, risky on a single-threaded ESP8266 with a very small TCP connection budget)
- Added retries for the stove serial number request specifically, since it's the one field that doesn't get periodically refreshed like the rest
- Reboot menu no longer gets stuck open on mobile (was relying on CSS `:hover` only, which has no "hover off" on touch)
- Manual firmware upload timeout increased (30s → 120s) and its error message is clearer when the connection just drops instead of returning a real error
- "Check for updates" now runs automatically on page load instead of only when opening the Firmware section
- Clearer Wi-Fi status wording ("Home network: Connected/Not connected" instead of raw `on`/`off`)

**Repo cleanup:**
- Removed adapter schematics, enclosure files, and the standalone build guide (`schematic/`, `box/`, `BUILD.md`) — this fork doesn't produce hardware; see upstream for those
- Removed the Lectronz store link/image — this fork doesn't sell anything

This is a personal fork tested only on my own Ferroli/Fumis Alpha setup — it hasn't gone through the review or testing that an upstream contribution would need.
