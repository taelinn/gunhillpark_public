# SEN66 Air Quality Sensor

Complete air quality monitoring using Sensirion SEN66 and Adafruit QT Py ESP32-S3.

## Installation

1. **Download** `sen66-qtpy.yaml` to your ESPHome folder
2. **Create** `secrets.yaml`:
```yaml
   wifi_ssid: "YOUR_WIFI_NAME"
   wifi_password: "YOUR_WIFI_PASSWORD"
```
3. **Install** via USB (click "Continue Anyway" if it shows "Unknown Device")
4. **Done!** Sensor appears in Home Assistant with unique name (air-quality-XXXXXX)

## Hardware
- [Adafruit QT Py ESP32-S3](https://www.adafruit.com/product/5426)
- [Adafruit SEN66 Breakout](https://www.adafruit.com/product/5958)
- STEMMA QT cable (no soldering!)

## Credits
Based on [uELKO's Air Quality Monitor](https://github.com/uELKO/ESPHome-Air-Quality-Monitor)