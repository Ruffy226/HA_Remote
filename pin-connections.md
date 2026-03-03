# HA Remote - TFT Display Pin Connections

## Display Module (2.4" TFT ILI9341 + XPT2046 Touch)

### TFT Display Pins (Left Side)

| TFT Pin | Function | ESP32-S3 GPIO | Notes |
|---------|----------|---------------|-------|
| VCC | Power | 3.3V or 5V | Check your display - most work with 3.3V logic |
| GND | Ground | GND | |
| CS | Chip Select | GPIO10 | SPI CS for display |
| RESET | Reset | GPIO14 | Or tie to 3.3V via 10K resistor |
| DC/RS | Data/Command | GPIO8 | Also called D/C or A0 |
| MOSI | SPI Data In | GPIO11 | SPI MOSI (Master Out) |
| SCK | SPI Clock | GPIO12 | SPI CLK |
| LED | Backlight | GPIO15 | PWM controllable via LEDC |
| MISO | SPI Data Out | GPIO13 | SPI MISO (optional for display) |

### Touch Panel Pins (Right Side)

| Touch Pin | Function | ESP32-S3 GPIO | Notes |
|-----------|----------|---------------|-------|
| T_CLK | Touch SPI Clock | GPIO12 | Shared with display SPI CLK |
| T_CS | Touch Chip Select | GPIO9 | Separate CS for touch controller |
| T_DIN | Touch Data In | GPIO11 | Shared with SPI MOSI |
| T_DO | Touch Data Out | GPIO13 | Shared with SPI MISO |
| T_IRQ | Touch Interrupt | GPIO16 | Optional - for touch detection |

### SPI Bus Sharing

The display and touch share the same SPI bus:
```
SPI CLK  (GPIO12) → SCK + T_CLK
SPI MOSI (GPIO11) → MOSI + T_DIN  
SPI MISO (GPIO13) → MISO + T_DO
```

Each device has its own Chip Select:
- Display CS: GPIO10
- Touch CS: GPIO9

---

## Quick Wiring Reference

```
ESP32-S3           TFT Display
────────           ───────────
GPIO12 ──────────── SCK + T_CLK
GPIO11 ──────────── MOSI + T_DIN
GPIO13 ──────────── MISO + T_DO
GPIO10 ──────────── CS (display)
GPIO9  ──────────── T_CS (touch)
GPIO8  ──────────── DC/RS
GPIO14 ──────────── RESET
GPIO15 ──────────── LED (backlight)
GPIO16 ──────────── T_IRQ (optional)
3.3V   ──────────── VCC
GND    ──────────── GND
```

---

## Optional: ADXL345 Accelerometer (Wake on Motion)

If using motion detection for wake:

| ADXL345 Pin | ESP32-S3 GPIO | Notes |
|-------------|---------------|-------|
| VCC | 3.3V | |
| GND | GND | |
| SDA | GPIO17 | I2C SDA |
| SCL | GPIO18 | I2C SCL |
| INT1 | GPIO16 | Motion interrupt (can share with T_IRQ if not using touch IRQ) |

---

## Notes

1. **Backlight Control**: GPIO15 drives the LED pin via PWM for brightness control and sleep/wake
2. **Reset Pin**: Can be tied to 3.3V with a 10K pullup if you don't need software reset
3. **Touch IRQ**: Optional - useful for detecting touch without polling. If unused, the XPT2046 can be polled via SPI
4. **Voltage**: Most 2.4" TFT modules are 3.3V logic compatible. Check your specific module
