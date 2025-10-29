# Athom Tech 15W RGBCT Bulb — ESPHome Template

## Overview
This ESPHome configuration is tailored for the **Athom Tech 15W RGBCT smart bulb**, an ESP8266-based light supporting full RGB + warm/cold white (CCT) control.  
It provides robust connectivity management, syslog integration, and numerous lighting effects, while ensuring safe operation even without Wi-Fi or API connectivity (“Dumb Mode”).

This template can be used as a drop-in configuration for any Athom Tech 15W RGBCT bulb running ESPHome 2025.9.0 or later.

---

## Features
- Full RGB + CCT control with color interlock  
- Dumb Mode failsafe for standalone operation if Wi-Fi or API are unavailable  
- Customizable lighting effects (Chill Mode, Disco, Lightning, Candle, Holiday themes, etc.)  
- Built-in connectivity diagnostics and syslog integration  
- Automatic flash preference sync and safe reconnect handling  
- Switch to toggle debug logging dynamically  
- UDP packet transport link support for remote control integration  

---

## Required Substitutions

| Name | Description |
|------|--------------|
| `name` | Device name (used as hostname) |
| `friendly_name` | Name shown in Home Assistant |
| `device_ip_address` | Static IP of the bulb |
| `device_ip_gateway` | Default gateway |
| `device_ip_subnet` | Subnet mask |
| `device_ip_dns` | DNS server |
| `syslog_ip_address` | IP address of your syslog receiver |
| `time_ntp_1` | Primary NTP server |
| `time_ntp_2` | Secondary NTP server |
| `time_ntp_3` | Tertiary NTP server |
| `master_switch_ip` | IP address of master switch device (for UDP provider) |
| `master_switch_provider` | Provider name for master switch packets |
| `master_switch_ota_entity` | Entity ID for master switch OTA detection |

---

## Optional Substitutions

| Name | Default | Description |
|------|----------|-------------|
| `default_log_level` | `WARN` | Default log verbosity |
| `dumb_mode_brightness` | `35%` | Brightness level when in Dumb Mode |
| `min_kelvin` | `2700K` | Warm white color temperature |
| `max_kelvin` | `6500K` | Cold white color temperature |

---

## Hardware Pin Mapping

| Function | GPIO | Notes |
|-----------|------|-------|
| Red channel | GPIO4 | PWM output |
| Green channel | GPIO12 | PWM output |
| Blue channel | GPIO14 | PWM output |
| White channel | GPIO5 | PWM output (non-inverted) |
| Cold white / CT | GPIO13 | PWM output (inverted) |

---

## Light Behavior

- On boot, the bulb waits 12 seconds for Wi-Fi/API to connect.  
- If both fail, it automatically enables **Dumb Mode** lighting (warm dimmed light).  
- Once Wi-Fi or API reconnects, it resumes normal operation.

---

## Available Light Effects

| Category | Effect Names |
|-----------|---------------|
| **Dynamic** | Random Slow, Random Fast, Flicker, Random Flicker |
| **Relaxed** | Chill Mode, Chill Mode (Vibrant), Breathing, RGB Breathing |
| **Party** | Disco, Lightning |
| **Seasonal** | Halloween, Halloween2, Halloween3, Christmas, Saint-Jean-Baptiste, Canada Day, Valentine |
| **Atmospheric** | Candle |

---

## Buttons & Diagnostics

| Entity | Description |
|---------|-------------|
| `Device Restart` | Restarts device normally |
| `Device Restart (Safe Mode)` | Boots into ESPHome safe mode |
| `Status` | Online/offline status |
| `Debug Log Level Enabled` | Enables verbose debugging for 24h |
| `WiFi SSID / IP / MAC / Channel` | Diagnostic sensors |

---

## Notes
- Flash writes are throttled to once every **2 hours** (`flash_write_interval: 2h`) to maximize flash lifespan.  
- Tested on **ESP-01M (ESP8266, 1MB)** modules used in Athom Tech 15W RGBCT bulbs.  
- Requires **ESPHome ≥ 2025.9.0**.  
- Supports full Home Assistant API control and OTA updates.

---

## Example Usage
```yaml
substitutions:
  name: athom-rgbct-livingroom
  friendly_name: Living Room Bulb
  device_ip_address: "192.168.0.50"
  device_ip_gateway: "192.168.0.1"
  device_ip_subnet: "255.255.255.0"
  device_ip_dns: "192.168.0.1"
  syslog_ip_address: "192.168.0.10"
  master_switch_ip: "192.168.0.100"
  master_switch_provider: "main_switch"
  master_switch_ota_entity: "binary_sensor.main_switch_shutdown"
  time_ntp_1: "0.ca.pool.ntp.org"
  time_ntp_2: "1.ca.pool.ntp.org"
  time_ntp_3: "2.ca.pool.ntp.org"
```

---

## Credits
Based on community ESPHome configurations adapted for **Athom Tech** hardware.  
Custom effects, safety logic, and network integration by **Benjamin Fiset-Deschênes**.
