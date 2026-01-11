# ESPHome IKEA VINDRIKTNING (ESP32 Edition)

Enhanced ESPHome firmware for the IKEA VINDRIKTNING air quality sensor,
based on the **LaskaKit ESP-VINDRIKTNING ESP32 I2C** board.

This project turns the VINDRIKTNING into a fully featured,
Home Assistant–integrated air quality station with LEDs, sensors,
alerts, and advanced automation hooks.

---

## ✨ Features

- 📊 **PM2.5 sensor (PM1006)**
- 🌡 **Temperature & Humidity (SCD4x)**
- 🫁 **CO₂ monitoring**
- 🌈 **3× WS2812 RGB status LEDs**
- 🔔 **Onboard piezo buzzer (GPIO15, PWM)**
- 🌞 **Day/Night brightness adjustment**
- 🎚 **User-configurable LED brightness cap**
- 🧠 **Fully local logic (no HA dependency for visuals)**
- 🔁 **Auto-restore state after reboot**
- 🔌 **ESPHome native API**

---

## 🧰 Hardware

| Component             | Notes                               |
| --------------------- | ----------------------------------- |
| Board                 | LaskaKit ESP-VINDRIKTNING ESP32 I2C |
| MCU                   | ESP32                               |
| PM Sensor             | PM1006                              |
| Temp / Humidity / CO₂ | Sensirion SCD4x                     |
| LEDs                  | 3× WS2812 RGB                       |
| Buzzer                | Passive piezo (GPIO15, PWM)         |

---

## 🌈 LED Status Logic

All 3 LEDs represent the current CO₂ level. The LEDs change color
progressively to indicate air quality at a glance.

| CO₂ Range (ppm) |       LED 0       |      LED 1       |       LED 2       | Notes                           |
| --------------: | :---------------: | :--------------: | :---------------: | :------------------------------ |
|          < 1000 |        🟢         |        🟢        |        🟢         | Good air — all green            |
|     1000 – 1499 |        🟢         |        🟢        |        🟠         | Beginning of elevated CO₂       |
|     1500 – 1999 |        🟢         |        🟠        |        🟠         | Moderate — two orange           |
|     2000 – 2499 |        🟠         |        🟠        |        🟠         | High — all orange               |
|     2500 – 2999 |        🔴         |        🟠        |        🟠         | Very high — one red, two orange |
|     3000 – 3499 |        🔴         |        🔴        |        🟠         | Dangerous — two red, one orange |
|     3500 – 3999 |        🔴         |        🔴        |        🔴         | Severe — all red                |
|          ≥ 4500 | 🔴 (pulsing high) | 🔴 (pulsing low) | 🔴 (pulsing high) | Critical — pulsing alert        |

Brightness is automatically reduced at night and capped by a
user-defined maximum value.

---

## 🔔 Buzzer

- Connected to **GPIO15**
- Driven using **ESP32 LEDC (PWM)**
- Supports different tones and alert patterns
- Intended for alerts (CO₂, PM2.5, etc.)

- **Buzzer**: The `Buzzer` is currently not working.
  > Note: True volume control is not possible due to hardware limitations.

---

## 🎚 Brightness Control

Maximum LED brightness is configurable via Home Assistant using an
`input_number`, synchronized into ESPHome using a global variable.

This avoids known ESPHome linker issues with `number.template`.

---

## 🏠 Home Assistant Integration

- Native ESPHome API
- Entities:
  - PM2.5
  - Temperature
  - Humidity
  - CO₂
  - WiFi signal
  - Status LEDs
  - Buzzer controls

---

## 🚀 Getting Started

1. Adjust pin assignments if needed
2. Flash ESPHome onto the ESP32
3. Enjoy a smarter VINDRIKTNING

---

## 🔗 Related Projects & References

- ESPHome  
  https://esphome.io/

- IKEA VINDRIKTNING teardown  
  https://github.com/Hypfer/esp8266-vindriktning-particle-sensor

- LaskaKit ESP-VINDRIKTNING board  
  https://www.laskakit.cz/

- Sensirion SCD4x  
  https://sensirion.com/products/catalog/SCD40/

---

---

## ⚠ Known Issues

- **Buzzer**: The `Buzzer` is currently not working.
- **Ambient light (lux)**: Lux readings are noisy; use the `Lux calibration` input in Home Assistant to apply a manual offset, or consider switching to Home Assistant's `sun` entity for day/night brightness adjustments instead of relying solely on local lux.

---

## 📄 License

MIT License — do whatever you want, just keep attribution.

---

## 🤝 Contributions

Issues, PRs, and improvements are welcome.
This project is intentionally modular and easy to extend.
