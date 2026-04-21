# 🚗 ECUMaster Black + ESP32 Bluetooth Display & Logger
### Real-time ECU data, alerts, and native ECUMaster logging - no laptop required

[![ESP32](https://img.shields.io/badge/board-ESP32-blue.svg)](https://www.espressif.com/en/products/socs/esp32)
[![ECUMaster](https://img.shields.io/badge/device-ECUMaster-black.svg)](https://www.ecumaster.com)
[![Bluetooth](https://img.shields.io/badge/communication-Bluetooth-brightgreen.svg)](https://en.wikipedia.org/wiki/Bluetooth)
[![LVGL](https://img.shields.io/badge/UI-LVGL-purple.svg)](https://lvgl.io/)
[![Arduino](https://img.shields.io/badge/framework-Arduino-blue.svg)](https://www.arduino.cc)
[![License: GPL v3](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Language: C++](https://img.shields.io/badge/language-C++-orange.svg)](https://isocpp.org)
[![Vehicle Telemetry](https://img.shields.io/badge/type-Telemetry-lightgrey.svg)]()
[![Data Logging](https://img.shields.io/badge/feature-Native%20Logging-green.svg)]()
[![RTC](https://img.shields.io/badge/feature-RTC%20Clock-orange.svg)]()

---

## 📦 Overview

This project connects an **ECUMaster Black ECU** to an **ESP32** over Bluetooth, creating a compact **standalone ECU display**.

> Designed for track use - reliable, offline, and zero laptop dependency.

- Real-time engine telemetry
- Driver alerts (visual + buzzer)
- Peak value tracking
- Automatic **native ECUMaster-compatible logging** with accurate RTC-based timestamps

---

## ✨ Core Features

### 🖥️ Live ECU Display
- Real-time ECU data over Bluetooth
- LVGL-based TFT interface (monospace fonts)
- Fast refresh with low latency
- Automatic Bluetooth reconnection

### 🚨 Alerts & Threshold Monitoring
- On-screen visual alerts
- Optional buzzer notifications
- CEL (Check Engine Light) warnings
- Configurable thresholds for:
  - Coolant temperature
  - RPM
  - AFR
  - Voltage
  - Boost / MAP
  - CEL status
  - Additional channels (see `Docs/Channel_list.pdf`)

Designed to **keep the driver informed without distraction**.

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

The ESP32 records logs directly to a microSD card in **native ECUMaster Black–compatible format**, with **accurate RTC-based timestamps**.

- Fully readable by **ECUMaster Black Software v2.x**
- No CSV conversion
- No external tools
- Same workflow as ECU-side logs
- **Correct file date/time via RTC (DS3231)**

### How it works
- Logging starts automatically on **engine start / ECU connection**
- A **new log file is created for each engine start**
- Each file is stamped with **real-world date and time**
- Files open directly in ECUMaster software on Windows 10/11

### Why this matters
- Laptop-free track days
- Long-term unattended logging
- Chronologically accurate logs for analysis
- Seamless analysis using official ECUMaster tools

---

## 💾 microSD Storage Management

Designed for long-term, hands-off use:

- Free space is continuously monitored
- When free space drops below **100 MB**:
  - The **oldest log file is automatically deleted**
- Logging continues uninterrupted - no full-card failures

---

## 🔒 Bluetooth Stability Improvements

- Incoming Bluetooth packets are **validated before parsing**
- Corrupted or partial frames are discarded safely
- Prevents freezes, crashes, or random reboots
- Handles reconnects cleanly during driving

Especially important in noisy RF environments or when reconnecting mid-drive.

---

## 🕒 RTC (Real-Time Clock)

- DS3231 RTC for accurate log timestamps and filenames
- Battery-backed (CR2032) - retains time when powered off
- Set once using `FORCE_RTC_UPDATE` (**true** → flash → **false** → reflash)

**Note:** Battery lasts ~2 years. Only needs resetting if the battery is removed or depleted.

---

## ✅ Tested On

- ESP32 JC2432W328
- Active 3.3V buzzer
- DS3231 (AT24C32) I2C RTC module
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
   - Wire
   - RTClib
3. Configure `lv_conf.h` and `TFT_eSPI` if needed
4. Set ECU Bluetooth **MAC ADDRESS or device name + PIN + uncomment `#define USE_NAME`**
5. Copy mono-space fonts:
   - `ui_font_JBM_18.c`
   - `ui_font_JBM_15.c`
   - `ui_font_JBM_10.c`
6. Insert a **FAT32-formatted** microSD card
7. Upload to ESP32
8. *(Optional)* Connect a 3.0–3.3V active buzzer to GPIO **16**
9. *(Optional)* Connect a RTC module to **I2C pins**
10. Pair with ECUMaster Black Bluetooth adapter
11. Drive 🚗 - logging starts automatically

---

## 📦 Dependencies

- [BluetoothSerial](https://github.com/espressif/arduino-esp32/tree/master/libraries/BluetoothSerial)
- [LVGL](https://lvgl.io)
- [TFT_eSPI](https://github.com/Bodmer/TFT_eSPI)
- [SPI](https://docs.arduino.cc/learn/communication/spi/)
- [SD](https://docs.arduino.cc/libraries/sd/)
- [Wire](https://github.com/esp8266/Arduino/blob/master/libraries/Wire/Wire.h)
- [RTCLIB](https://github.com/adafruit/rtclib)

---

## ❓ FAQ

**Q:** Display not initializing?  
**A:** Check TFT_eSPI and LVGL configuration.

**Q:** Logs not recording?  
**A:** Verify microSD wiring, FAT32 format, and SPI pins.

**Q:** Blurry text?  
**A:** Check `LV_COLOR_16_SWAP` in `lv_conf.h`.

**Q:** Recommended 3D printing material?  
**A:** ABS or ASA - **suitable for high cabin temperatures**.

**Q:** MicroSD card does not work?<br>
**A:** Re-format the memory card to FAT32. If it still does not work, try a smaller capacity card (8GB)

**Q:** Filenames have incorrect timestamps<br>
**A:** Force an RTC sync:
1. Set `#define FORCE_RTC_UPDATE true` and flash the ESP32  
2. Then set `#define FORCE_RTC_UPDATE false` and flash again  

This updates the RTC with the correct compile-time and restores accurate timestamps.
> Note: This only needs to be done once unless the RTC loses power.

---

## 🧰 Hardware Used

- **ESP32 JC2432W328**<br>
  https://www.aliexpress.com/item/1005006729707613.html

- **3D Printed Case #1 for ESP32**<br>
  https://www.thingiverse.com/thing:6705691

- **3D Printed Case #2 for RTC**<br> 
  https://drive.google.com/drive/folders/1Sk4sIXgLAqPZ03BzYb0IwUftMXJ9QMLN?usp=sharing

- **EMU Black (ECU Master Black)**<br>
  https://www.ecumaster.com/products/emu-black/
  
- **EMU Black CAN Bus Bluetooth Adapter**<br> 
  https://ecumasterusa.com/products/bluetooth-adapter-for-ecumaster-emu-can-bus

- **Active 3.3V Buzzer**<br>
  https://www.aliexpress.com/item/1005008682347898.html

- **MicroSD card (8GB)**<br>
  https://www.amazon.com/Micro-Sd-Card-8gb/s?k=Micro+Sd+Card+8gb
  
- **DS3231 I2C RTC Module**<br>
  https://www.aliexpress.com/item/1005003707505154.html
 
---

## 📺 Demo

![Display](https://raw.githubusercontent.com/danuecumaster/ECUMaster-Black-ESP32-Bluetooth-Dashboard-Logger/main/Docs/display.jpg)

[YouTube](https://youtu.be/EzVEeiy3vmI)

---

## 📝 Notes on Log Filenames (v8 Change)

- **v7** uses sequential filenames (e.g. `0001.emualog`, `0002.emualog`) and does **not require an RTC module**.
- **v8** uses **RTC-based timestamps** to generate filenames with real-world date and time.
- If you prefer a simpler setup without RTC dependency, v7 remains a valid option.

---

## 📝 Notes on Log Analysis (v7 Change)

- The **custom online log analyzer used in v6 has been removed**
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
