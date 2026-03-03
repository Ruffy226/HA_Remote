# HA Remote - Wiring Diagram

Based on LCD Wiki MSP2402 2.4" SPI ILI9341 module standard pinout.

## Display SPI Header (9 pins)

| Pin | Label | Function | ESP32-S3 GPIO |
|-----|-------|----------|---------------|
| 1 | VCC | Power | 3.3V |
| 2 | GND | Ground | GND |
| 3 | CS | LCD Chip Select | GPIO10 |
| 4 | RESET | LCD Reset | GPIO14 |
| 5 | DC/RS | Data/Command | GPIO8 |
| 6 | SDI (MOSI) | SPI Data In | GPIO11 |
| 7 | SCK | SPI Clock | GPIO12 |
| 8 | LED | Backlight | GPIO15 |
| 9 | SDO (MISO) | SPI Data Out | GPIO13 |

## Touch SPI Header (5 pins)

| Pin | Label | Function | ESP32-S3 GPIO |
|-----|-------|----------|---------------|
| 1 | T_CLK | Touch Clock | GPIO12 (shared with SCK) |
| 2 | T_CS | Touch Chip Select | GPIO9 |
| 3 | T_DIN | Touch Data In | GPIO11 (shared with MOSI) |
| 4 | T_DO | Touch Data Out | GPIO13 (shared with MISO) |
| 5 | T_IRQ | Touch Interrupt | GPIO16 |

## SPI Bus Sharing

Display and touch share the same SPI bus:
- **SCK** (GPIO12) → Display pin 7 + Touch T_CLK
- **MOSI** (GPIO11) → Display pin 6 + Touch T_DIN
- **MISO** (GPIO13) → Display pin 9 + Touch T_DO

Each device has its own chip select:
- **Display CS**: GPIO10
- **Touch CS**: GPIO9

## Quick Wiring Reference

```
ESP32-S3              Display Module
────────              ──────────────
3.3V    ────────────── VCC (pin 1)
GND     ────────────── GND (pin 2)
GPIO10  ────────────── CS (pin 3)
GPIO14  ────────────── RESET (pin 4)
GPIO8   ────────────── DC/RS (pin 5)
GPIO11  ────────────── SDI/MOSI (pin 6)
GPIO12  ────────────── SCK (pin 7)
GPIO15  ────────────── LED (pin 8)
GPIO13  ────────────── SDO/MISO (pin 9)

ESP32-S3              Touch Header
────────              ────────────
GPIO9   ────────────── T_CS
GPIO16  ────────────── T_IRQ
(GPIO11, 12, 13 shared with display SPI)
```

## ADXL345 Accelerometer (Optional)

| ADXL345 | ESP32-S3 |
|---------|----------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO17 |
| SCL | GPIO18 |
| INT1 | GPIO21 |
| CS | 3.3V (I2C mode) |

---

## Notes

1. **Backlight**: LED pin can be PWM controlled via GPIO15 for brightness and sleep/wake
2. **Touch IRQ**: T_IRQ goes LOW when touch detected - useful for wake-on-touch
3. **MISO**: Optional if you don't need to read from display (touch still needs it)
4. **Voltage**: Module works with 3.3V logic (VCC can be 3.3V or 5V)
