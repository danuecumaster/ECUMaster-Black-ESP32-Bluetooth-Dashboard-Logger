# 🚗 ECUMaster Black + ESP32 Bluetooth Display & Logger
### Real-time ECU data, alerts, and native ECUMaster logging

[![ESP32](https://img.shields.io/badge/board-ESP32-blue.svg)](https://www.espressif.com/en/products/socs/esp32)
[![ECUMaster](https://img.shields.io/badge/device-ECUMaster-black.svg)](https://www.ecumaster.com)
[![Bluetooth](https://img.shields.io/badge/communication-Bluetooth-brightgreen.svg)](https://en.wikipedia.org/wiki/Bluetooth)
[![LVGL](https://img.shields.io/badge/UI-LVGL-purple.svg)](https://lvgl.io/)
[![Arduino](https://img.shields.io/badge/framework-Arduino-blue.svg)](https://www.arduino.cc)
[![License: GPL v3](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Language: C++](https://img.shields.io/badge/language-C++-orange.svg)](https://isocpp.org)
[![Vehicle Telemetry](https://img.shields.io/badge/type-Telemetry-lightgrey.svg)]()
[![Data Logging](https://img.shields.io/badge/feature-Native%20Logging-green.svg)]()

---

## 📦 Overview

This project connects an **ECUMaster Black ECU** to an **ESP32** over Bluetooth, creating a compact **standalone ECU display** with:

- Live engine telemetry
- Driver alerts (visual + buzzer)
- Peak value tracking
- Automatic **native ECUMaster-compatible logging**

---

## ✨ Core Features

### 🖥️ Live ECU Display
- Real-time ECU data via Bluetooth
- LVGL-based TFT GUI (mono-space fonts)
- Fast refresh, low latency
- Automatic Bluetooth reconnect

### 🚨 Alerts & Threshold Monitoring
- Visual alerts on screen
- Optional buzzer alerts
- CEL warnings
- Configurable thresholds for:
  - Coolant temperature
  - RPM
  - AFR
  - Voltage
  - Boost / MAP
  - CEL status

Designed to **keep the driver informed**, not distracted.

---

## 📊 Peak Value Tracking

The display tracks and shows the **highest values since power-on**:

- **Peak Boost Pressure**
- **Peak Coolant Temperature (CLT)**

Useful for:
- Track sessions
- Safety checks
- Quick post-drive review without opening logs

---

## 📀 Native ECUMaster-Compatible Logging

The ESP32 records logs directly to a microSD card in **native ECUMaster Black–compatible format**.

- Fully readable by **ECUMaster Black Software v2.x**
- No CSV conversion
- No external tools
- Same workflow as ECU-side logs

### How it works
- Logging starts automatically on **engine start / ECU connection**
- A **new log file is created for each engine start**
- Files open directly in ECUMaster software on Windows 10/11

### Why this matters
- Laptop-free track days
- Long-term unattended logging
- Seamless analysis using official ECUMaster tools

---

## 💾 microSD Storage Management

Designed for long-term, hands-off use:

- Free space is continuously monitored
- When free space drops below **100 MB**:
  - The **oldest log file is automatically deleted**
- Logging never stops due to a full card

---

## 🔒 Bluetooth Stability Improvements

- Incoming Bluetooth packets are **validated before parsing**
- Corrupted or partial frames are discarded safely
- Prevents freezes, crashes, or random reboots
- Handles reconnects cleanly during driving

Especially important in noisy RF environments or when reconnecting mid-drive.

---

## ✅ Tested On

- ESP32 JC2432W328
- Active 3.3V buzzer
- Arduino Core **v2.0.17**
- ECUMaster Black + Bluetooth Adapter
- ECUMaster Black Software **v2.x** (Windows 10 / 11)
- LVGL v8.3
- 8GB microSD card (FAT32)

> ⚠️ **Note:**  
> If `SerialBT.setPin(pin)` does not work, downgrade to **ESP32 Arduino Core 2.0.17**.  
> Newer cores removed this feature.

---

## 📥 Installation

1. Clone this repository
2. Install dependencies:
   - LVGL
   - TFT_eSPI
   - BluetoothSerial
   - SD
3. Configure `lv_conf.h` and `TFT_eSPI` if needed
4. Set ECU Bluetooth **MAC or device name + PIN**
5. Copy mono-space fonts:
   - `ui_font_JBM_18.c`
   - `ui_font_JBM_15.c`
   - `ui_font_JBM_10.c`
6. Insert a **FAT32-formatted** microSD card
7. Upload to ESP32
8. *(Optional)* Connect a 3.0–3.3V active buzzer to GPIO **22**
9. Pair with ECUMaster Black Bluetooth adapter
10. Drive 🚗 — logging starts automatically

---

## 📦 Dependencies

- [BluetoothSerial](https://github.com/espressif/arduino-esp32/tree/master/libraries/BluetoothSerial)
- [LVGL](https://lvgl.io)
- [TFT_eSPI](https://github.com/Bodmer/TFT_eSPI)
- [SPI](https://docs.arduino.cc/learn/communication/spi/)
- [SD](https://docs.arduino.cc/libraries/sd/)

---

## ❓ FAQ

**Q:** Display not initializing?  
**A:** Check TFT_eSPI and LVGL configuration.

**Q:** Logs not recording?  
**A:** Verify microSD wiring, FAT32 format, and SPI pins.

**Q:** Blurry text?  
**A:** Check `LV_COLOR_16_SWAP` in `lv_conf.h`.

**Q:** Recommended 3D printing material?  
**A:** ABS or ASA (tested for high cabin temperatures).

---

## 🧰 Hardware Used

- **ESP32 JC2432W328**  
  https://www.aliexpress.com/item/1005006729707613.html

- **3D Printed Case**  
  https://www.thingiverse.com/thing:6705691

- **ECU**  
  ECUMaster Black + Bluetooth Adapter

- **Active 3.3V Buzzer**  
  https://www.aliexpress.com/item/1005008682347898.html

---

## 📺 Demo

![Display](Docs/display.jpg)

[https://youtu.be/EzVEeiy3vmI] (https://youtu.be/EzVEeiy3vmI)

---

## 📝 Notes on Log Analysis (v5 Change)

- The **custom online log analyzer used in v5 has been removed**
- Logs are now written in **native ECUMaster HEX format**
- Use **ECUMaster Black Software v2.x** for:
  - Log viewing
  - Channel math
  - Overlays & comparisons

This simplifies the workflow and keeps analysis fully within the official ECUMaster ecosystem.

---

## 📜 License

Licensed under the **GNU General Public License v3.0**. See [LICENSE](https://github.com/danuecumaster/ECUMaster-Black-ESP32-Bluetooth-Dashboard-Logger/wiki/LICENSE).

---

## ❤️ Credits & Contributions

Made with ❤️ for petrolheads.

Forks, pull requests, and feature requests are welcome.