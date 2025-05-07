Here's a concise **ESP32-WROOM-32 Hardware Cheat Sheet** for quick reference:

---

### **Core Specifications**  
- **Cores:** Dual-core Xtensa LX6 @ 240MHz  
- **Memory:** 520KB SRAM, 4MB Flash  
- **Wireless:** Wi-Fi 802.11 b/g/n (2.4GHz), Bluetooth 4.2 (BLE + Classic)  
- **Voltage:** 3.3V (IO pins), 5V input via `VIN`  
- **Current Limits:** 40mA per GPIO, 1200mA max for chip  

---

### **Pinout Quick Reference**  

#### **GPIO Pins**  
- **Digital I/O:** All GPIOs (except 6-11, 34-39).  
- **PWM:** Most GPIOs (use `ledc` library).  
- **Input-Only (No Pull-Up):** GPIO34-39.  

#### **Analog Pins**  
- **ADC (12-bit):** GPIO32-39 (8 channels, 0-3.3V).  
- **DAC (8-bit):** GPIO25, GPIO26.  

#### **Communication**  
| Protocol | Default Pins       | Alternate Pins | Max Speed      |  
|----------|--------------------|----------------|----------------|  
| **UART** | TX=GPIO1, RX=GPIO3 | Any GPIO       | 5 Mbps         |  
| **SPI**  | MOSI=23, MISO=19, SCK=18, CS=5 | Configurable   | 80 MHz         |  
| **I2C**  | SDA=21, SCL=22     | Any GPIO       | 1 MHz (Fast Mode) |  

#### **Special Pins**  
- **Touch Sensors:** GPIO2, 4, 12-15, 27-33.  
- **RTC (Deep Sleep):** GPIO0, 2, 4, 12-15, 25-27, 32-39.  
- **Boot Mode:** 
  - `GPIO0` (LOW = Boot Mode).  
  - `EN` (Reset: Pull LOW to reset).  

#### **Power Pins**  
- `3V3`: 3.3V output (max 600mA).  
- `VIN`: 5V input (regulated to 3.3V).  
- `GND`: Ground.  

#### **Restricted Pins**  
- **Do NOT use:** GPIO6-11 (connected to flash memory).  
- **NC Pins:** Leave unconnected.  

---

### **Key Libraries**  
```cpp
#include <WiFi.h>         // Wi-Fi  
#include <Bluetooth.h>    // Bluetooth Classic  
#include <BLE.h>          // BLE  
#include <SPI.h>          // SPI  
#include <Wire.h>         // I2C  
#include <driver/adc.h>   // Advanced ADC  
```

---

### **Power Management**  
- **Deep Sleep Current:** ~10μA.  
- Use `esp_deep_sleep_start()` to enter deep sleep.  
- Wakeup Sources: Timer, Touch, External (GPIO).  

---

### **Common Pitfalls**  
1. **ADC Noise:** Use `esp_adc_cal_characterize()` for calibration.  
2. **Floating Pins:** Enable internal pull-up/down:  
   ```cpp  
   pinMode(pin, INPUT_PULLUP);  
   ```  
3. **5V Devices:** Always use a logic level converter.  

---

### **Example Code Snippets**  

#### Blink LED (GPIO2):  
```cpp 
void setup() { pinMode(2, OUTPUT); }  
void loop() { digitalWrite(2, !digitalRead(2)); delay(500); }  
```  

#### Read ADC (GPIO34):  
```cpp 
void setup() { Serial.begin(115200); }  
void loop() { Serial.println(analogRead(34)); delay(100); }  
```  

#### PWM (GPIO13):  
```cpp 
const int freq = 5000;  
const int channel = 0;  
const int resolution = 8;  
void setup() { ledcSetup(channel, freq, resolution); ledcAttachPin(13, channel); }  
void loop() { for(int duty=0; duty<=255; duty++){ ledcWrite(channel, duty); delay(10); }}  
```  

---

### **Resources**  
- [ESP32 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf)  
- [Arduino-ESP32 GitHub](https://github.com/espressif/arduino-esp32)  

Keep this sheet handy for wiring and debugging! 🔧🚀
