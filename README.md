# HA Remote - ESPHome Touch Controller

A 4-button touchscreen remote for Home Assistant using ESP32-S3 with a 2.4" TFT display.

## Hardware

- **MCU**: ESP32-S3 N16R8 (16MB flash, 8MB PSRAM)
- **Display**: 2.4" TFT LCD Shield with ST7789V driver (240x320)
- **Touch**: XPT2046 resistive touchscreen

## Controls

| Button | Entity | Action |
|--------|--------|--------|
| Joe's Lamp | `light.0xb0ce1814030b1eb3` | Toggle on/off |
| Sara's Lamp | `light.0xb0ce1814030b3ff1` | Toggle on/off |
| Goodnight | `script.goodnight_script` | Trigger scene |
| Bedroom Fan | `fan.in_wall_fan_speed_control_300s` | Toggle on/off |

## Features

- Real-time state display (ON/OFF indicators)
- Color-coded buttons (green = on, gray = off)
- 2-second auto-refresh for state changes
- Touch-responsive UI

## Wiring (ESP32-S3 DevKitC-1)

| Pin | Function |
|-----|----------|
| GPIO18 | SPI CLK |
| GPIO23 | SPI MOSI |
| GPIO19 | SPI MISO |
| GPIO5  | Display CS |
| GPIO4  | Display DC |
| GPIO22 | Display Reset |
| GPIO21 | Touch CS |
| GPIO27 | Touch IRQ |
| GPIO48 | Status LED (built-in, inverted) |

## Setup

1. Copy `secrets.yaml.example` to `secrets.yaml` and fill in credentials
2. Compile and flash:
   ```bash
   esphome run ha-remote.yaml
   ```
3. Add the device to Home Assistant (Settings > Devices > ESPHome)

## License

MIT
