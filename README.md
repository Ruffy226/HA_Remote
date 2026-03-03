# HA Remote - ESPHome Touch Controller

A 4-button touchscreen remote for Home Assistant using ESP32-S3 with a 2.4" TFT display and ADXL345 accelerometer for sleep/wake detection.

## Hardware

- **MCU**: ESP32-S3 N16R8 (16MB flash, 8MB PSRAM)
- **Display**: 2.4" TFT LCD Shield with ILI9341 driver (240x320)
- **Touch**: XPT2046 resistive touchscreen
- **Motion**: ADXL345 3-axis accelerometer (sleep/wake)

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
- **Sleep/wake via ADXL345**: Display turns off after 30s idle, wakes on motion or touch
- Touch-responsive UI

---

## Wiring

### TFT Display (Left Header)

| TFT Pin | Function | ESP32-S3 GPIO |
|---------|----------|---------------|
| VCC | Power | 3.3V |
| GND | Ground | GND |
| CS | Chip Select | GPIO10 |
| RESET | Reset | GPIO14 |
| DC/RS | Data/Command | GPIO8 |
| MOSI | SPI Data In | GPIO11 |
| SCK | SPI Clock | GPIO12 |
| LED | Backlight | GPIO15 (PWM) |
| MISO | SPI Data Out | GPIO13 |

### Touch Panel (Right Header)

| Touch Pin | Function | ESP32-S3 GPIO |
|-----------|----------|---------------|
| T_CLK | SPI Clock | GPIO12 (shared) |
| T_CS | Chip Select | GPIO9 |
| T_DIN | SPI Data In | GPIO11 (shared) |
| T_DO | SPI Data Out | GPIO13 (shared) |
| T_IRQ | Interrupt | GPIO16 (optional) |

### SPI Bus

Display and touch share the SPI bus:
```
GPIO12 → SCK + T_CLK
GPIO11 → MOSI + T_DIN
GPIO13 → MISO + T_DO
```

Each device has its own CS pin:
- Display: GPIO10
- Touch: GPIO9

### ADXL345 Accelerometer (I2C)

| ADXL Pin | ESP32-S3 GPIO |
|----------|---------------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO17 |
| SCL | GPIO18 |
| INT1 | GPIO16 (motion interrupt) |
| CS | 3.3V (enables I2C mode) |

---

## Quick Reference

```
                ESP32-S3
              ┌─────────┐
     3.3V ────┤ VCC     ├──── 3.3V (display/touch/accel)
      GND ────┤ GND     ├──── GND
              │         │
   GPIO12 ────┤ SCK     ├───┐
   GPIO11 ────┤ MOSI    ├───┼─── SPI Bus (shared)
   GPIO13 ────┤ MISO    ├───┘
              │         │
   GPIO10 ────┤ CS      ├─── Display CS
    GPIO9 ────┤ T_CS    ├─── Touch CS
    GPIO8 ────┤ DC      ├─── Display DC
   GPIO14 ────┤ RST     ├─── Display Reset
   GPIO15 ────┤ LED     ├─── Backlight PWM
   GPIO16 ────┤ T_IRQ   ├─── Touch IRQ / ADXL INT1
              │         │
   GPIO17 ────┤ SDA     ├─── I2C (ADXL345)
   GPIO18 ────┤ SCL     ├─── I2C (ADXL345)
              └─────────┘
```

---

## Setup

1. Copy `secrets.yaml.example` to `secrets.yaml` and fill in credentials
2. Compile and flash:
   ```bash
   esphome run ha-remote.yaml
   ```
3. Add the device to Home Assistant (Settings > Devices > ESPHome)

## Files

- `ha-remote.yaml` - Main ESPHome configuration
- `pin-connections.md` - Detailed pin wiring reference
- `WIRING.md` - Additional wiring notes
- `secrets.yaml.example` - Template for WiFi/API credentials

## License

MIT
