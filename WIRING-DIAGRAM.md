# HA Remote - Wiring Diagram

## Display Module Pin Headers (Actual)

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│   J1 (Display - 9 pins)      J2 (Touch - 5 pins)             │
│   ┌─────────────┐            ┌─────────┐                     │
│   │ 1  VCC      │            │ T_IRQ   │                     │
│   │ 2  GND      │            │ T_DO    │                     │
│   │ 3  LCD_CS   │            │ T_DIN   │                     │
│   │ 4  RESET    │            │ T_CS    │                     │
│   │ 5  DC/RS    │            │ T_CLK   │                     │
│   │ 6  LED_A    │            └─────────┘                     │
│   │ 7  MISO     │                                            │
│   │ 8  SCK      │            J3 (SD Card - 6 pins)           │
│   │ 9  MOSI     │            ┌─────────┐                     │
│   └─────────────┘            │ SD_SS   │                     │
│                              │ SD_DI   │                     │
│                              │ SD_DO   │                     │
│                              │ SD_SCK  │                     │
│                              │ LCD_RST │ ← Not needed, use   │
│                              │ MISO    │   J1 RESET instead  │
│                              └─────────┘                     │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## J1 (Display - 9 pins) → ESP32-S3

| J1 Pin | Label | ESP32-S3 | Notes |
|--------|-------|----------|-------|
| 1 | VCC | 3.3V | Power |
| 2 | GND | GND | Ground |
| 3 | LCD_CS | GPIO10 | Display chip select |
| 4 | RESET | GPIO14 | Display reset |
| 5 | DC/RS | GPIO8 | Data/Command |
| 6 | LED_A | GPIO15 | Backlight PWM |
| 7 | MISO | GPIO13 | SPI MISO |
| 8 | SCK | GPIO12 | SPI Clock |
| 9 | MOSI | GPIO11 | SPI MOSI |

---

## J2 (Touch - 5 pins) → ESP32-S3

| J2 Pin | Label | ESP32-S3 | Notes |
|--------|-------|----------|-------|
| 1 | T_IRQ | GPIO16 | Touch interrupt |
| 2 | T_DO | GPIO13 | Shares MISO (internally connected) |
| 3 | T_DIN | GPIO11 | Shares MOSI (internally connected) |
| 4 | T_CS | GPIO9 | Touch chip select |
| 5 | T_CLK | GPIO12 | Shares SCK (internally connected) |

**Note**: T_DO, T_DIN, T_CLK are internally connected to J1's MISO, MOSI, SCK. Only need to wire T_CS and T_IRQ.

---

## J3 (SD Card - 6 pins) → ESP32-S3

| J3 Pin | Label | Wire? | Notes |
|--------|-------|-------|-------|
| 1 | SD_SS | No | SD card - skip |
| 2 | SD_DI | No | SD MOSI - skip |
| 3 | SD_DO | No | SD MISO - skip |
| 4 | SD_SCK | No | SD Clock - skip |
| 5 | LCD_RST | No | Use J1 RESET instead |
| 6 | MISO | No | Use J1 MISO instead |

**Skip J3 entirely** - all needed pins are on J1.

---

## Quick Reference

```
J1 (Display - 9 pins)     ESP32-S3
─────────────────────     ────────
VCC     ────────────────── 3.3V
GND     ────────────────── GND
LCD_CS  ────────────────── GPIO10
RESET   ────────────────── GPIO14
DC/RS   ────────────────── GPIO8
LED_A   ────────────────── GPIO15
MISO    ────────────────── GPIO13
SCK     ────────────────── GPIO12
MOSI    ────────────────── GPIO11

J2 (Touch - 5 pins)       ESP32-S3
─────────────────────     ────────
T_IRQ   ────────────────── GPIO16
T_CS    ────────────────── GPIO9
(T_DO, T_DIN, T_CLK shared via J1)

J3 (SD Card) - Skip entirely
```

---

## ADXL345 Accelerometer (Optional)

| ADXL345 | ESP32-S3 |
|---------|----------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO17 |
| SCL | GPIO18 |
| INT1 | GPIO21 |
| CS | 3.3V (I2C mode) |
