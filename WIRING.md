# HA Remote Wiring Guide

## ESP32-S3 N16R8 DevKitC-1 Pinout

### Display & Touch (SPI Bus)

| TFT Shield Pin | ESP32-S3 GPIO | Notes |
|----------------|---------------|-------|
| LCD_CS | GPIO5 | Display chip select |
| LCD_DC | GPIO4 | Data/Command |
| LCD_RST | GPIO22 | Display reset |
| TOUCH_CS | GPIO21 | Touch chip select |
| TOUCH_IRQ | GPIO27 | Touch interrupt |
| SCK | GPIO18 | SPI clock |
| MOSI | GPIO23 | SPI data (MCU→device) |
| MISO | GPIO19 | SPI data (device→MCU) |
| VCC | 3.3V or 5V | Check shield specs |
| GND | GND | Common ground |

### ADXL345 Accelerometer (I2C Bus)

| ADXL345 Pin | ESP32-S3 GPIO | Notes |
|-------------|---------------|-------|
| VCC | 3.3V | Do NOT use 5V |
| GND | GND | Common ground |
| SDA | GPIO8 | I2C data |
| SCL | GPIO9 | I2C clock |
| CS | 3.3V | Tie high for I2C mode |
| SDO | GND or 3.3V | Sets I2C address (0x53 or 0x1D) |

### Built-in

| Function | GPIO | Notes |
|----------|------|-------|
| Status LED | GPIO48 | Inverted (active-low) |

---

## Power Options

### USB (Recommended for Development)
- USB-C from PC or 5V charger
- Powers both ESP32 and TFT shield

### Battery (Future)
- LiPo via JST connector (if board has one)
- Or 3.7V LiPo + TP4056 charger module
- Deep sleep mode extends battery life significantly

---

## SPI Bus Sharing

The display and touchscreen share one SPI bus:
- Each has its own CS (Chip Select) pin
- Only one device communicates at a time
- ADXL345 uses I2C (separate bus), no conflict

---

## Common Issues

| Symptom | Likely Cause |
|---------|--------------|
| Display white/blank | Wrong CS/DC pins, or power issue |
| Touch not responding | Check TOUCH_CS and IRQ pins |
| Accelerometer not found | Check I2C wiring, CS must be HIGH |
| Compile error | Ensure `psram: octal` matches your board |

---

## Verification Steps

1. **Flash firmware** — check serial monitor for boot messages
2. **Display test** — should show button UI within 2 seconds
3. **Touch test** — tap buttons, watch serial output for presses
4. **Motion test** — pick up remote, display should wake if sleeping

---

## Pin Reference (Quick)

```
ESP32-S3 N16R8
┌─────────────────────┐
│  GPIO48 ── LED      │ (built-in)
│  GPIO18 ── SCK      │──┐
│  GPIO23 ── MOSI     │  │ SPI Bus
│  GPIO19 ── MISO     │  │ (Display + Touch)
│  GPIO5  ── LCD_CS   │──┘
│  GPIO4  ── LCD_DC   │
│  GPIO22 ── LCD_RST  │
│  GPIO21 ── TOUCH_CS │
│  GPIO27 ── TOUCH_IRQ│
│                     │
│  GPIO8  ── SDA      │──┐ I2C Bus
│  GPIO9  ── SCL      │──┘ (ADXL345)
└─────────────────────┘
```
