# HA Remote - Wiring Diagram

## Display Module Pin Headers (Actual)

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│   J3 (SD + Reset)          J1 (Display)     J2 (Touch)       │
│   ┌──────────────┐         ┌─────────┐      ┌─────┐          │
│   │ SD_SS        │         │ VCC     │      │T_IRQ│          │
│   │ SD_DI        │         │ GND     │      │T_DO │          │
│   │ SD_DO        │         │ CS      │      │T_DIN│          │
│   │ SD_SCK       │         │ RESET*  │      │T_CS │          │
│   │ LCD_RST      │         │ DC/RS   │      │T_CLK│          │
│   │ MISO         │         │ SDA     │      └─────┘          │
│   └──────────────┘         │ SCL     │                       │
│                            │ LED     │                       │
│                            └─────────┘                       │
│                                                              │
│   *RESET on J1 is SD card reset, not LCD reset               │
│   LCD_RST is on J3!                                          │
│   SDA = MOSI (SPI data), SCL = SCK (SPI clock)               │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## J1 (Display Power + Control) → ESP32-S3

| J1 Pin | Label | ESP32-S3 | Notes |
|--------|-------|----------|-------|
| 1 | VCC | 3.3V | Power |
| 2 | GND | GND | Ground |
| 3 | CS | GPIO10 | Display chip select |
| 4 | RESET* | NC | SD card reset - ignore |
| 5 | DC/RS | GPIO8 | Data/Command |
| 6 | SDA | GPIO11 | **SPI MOSI** (labeled SDA) |
| 7 | SCL | GPIO12 | **SPI SCK** (labeled SCL) |
| 8 | LED | GPIO15 | Backlight PWM |

*Note: RESET on J1 is for SD card. LCD reset is on J3!

---

## J3 (SD Card + LCD Reset + MISO) → ESP32-S3

| J3 Pin | Label | ESP32-S3 | Notes |
|--------|-------|----------|-------|
| 1 | SD_SS | - | SD card - skip |
| 2 | SD_DI | - | SD MOSI - skip |
| 3 | SD_DO | - | SD MISO - skip |
| 4 | SD_SCK | - | SD Clock - skip |
| 5 | **LCD_RST** | **GPIO14** | **Display reset** |
| 6 | **MISO** | **GPIO13** | SPI MISO |

---

## J2 (Touch) → ESP32-S3

| J2 Pin | Label | ESP32-S3 | Notes |
|--------|-------|----------|-------|
| 1 | T_IRQ | GPIO16 | Touch interrupt |
| 2 | T_DO | GPIO13 | Shares MISO (may be internally connected) |
| 3 | T_DIN | GPIO11 | Shares MOSI (may be internally connected) |
| 4 | T_CS | GPIO9 | Touch chip select |
| 5 | T_CLK | GPIO12 | Shares SCK (may be internally connected) |

---

## Complete Wiring Table

### J1 (Display)

| Pin | Label | Wire To | Notes |
|-----|-------|---------|-------|
| 1 | VCC | 3.3V | Power |
| 2 | GND | GND | Ground |
| 3 | CS | GPIO10 | Chip select |
| 4 | RESET | NC | SD card reset |
| 5 | DC/RS | GPIO8 | Data/Command |
| 6 | SDA | GPIO11 | SPI MOSI |
| 7 | SCL | GPIO12 | SPI SCK |
| 8 | LED | GPIO15 | Backlight |

### J3 (LCD Reset + MISO)

| Pin | Label | Wire To |
|-----|-------|---------|
| 5 | **LCD_RST** | **GPIO14** |
| 6 | **MISO** | **GPIO13** |

### J2 (Touch)

| Pin | Label | Wire To |
|-----|-------|---------|
| 1 | T_IRQ | GPIO16 |
| 4 | T_CS | GPIO9 |

*(T_CLK, T_DIN, T_DO likely shared via J1 - verify with multimeter)*

---

## Quick Reference

```
J1 (Display)              ESP32-S3
────────────              ────────
VCC     ────────────────── 3.3V
GND     ────────────────── GND
CS      ────────────────── GPIO10
DC/RS   ────────────────── GPIO8
SDA     ────────────────── GPIO11 (SPI MOSI)
SCL     ────────────────── GPIO12 (SPI SCK)
LED     ────────────────── GPIO15

J3 (Reset + MISO)         ESP32-S3
─────────────────         ────────
LCD_RST ────────────────── GPIO14
MISO    ────────────────── GPIO13

J2 (Touch)               ESP32-S3
────────────              ────────
T_CS    ────────────────── GPIO9
T_IRQ   ────────────────── GPIO16
```

---

## ADXL345 Accelerometer (Optional)

| ADXL345 | ESP32-S3 |
|---------|----------|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO17 |
| SCL | GPIO18 |
| INT1 | GPIO21* |
| CS | 3.3V (I2C mode) |

*Moved INT1 to GPIO21 since GPIO16 is used for T_IRQ.
