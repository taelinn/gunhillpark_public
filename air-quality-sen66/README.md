# SEN66 Air Quality Sensor

Comprehensive air quality monitoring using Sensirion SEN66.

Based on [uELKO's ESPHome Air Quality Monitor](https://github.com/uELKO/ESPHome-Air-Quality-Monitor), adapted for Adafruit QT Py ESP32-S3.

## Features
- **Particulate Matter**: PM1.0, PM2.5, PM4.0, PM10 measurements
- **CO2 Monitoring**: 400-5000 ppm range
- **Environmental**: Temperature & humidity
- **Air Quality**: VOC and NOx indices
- **Visual Feedback**: Color-coded status LED on QT Py
- **Web Dashboard**: Built-in web interface for local monitoring
- **Air Quality Level**: Automatic classification (Good/Moderate/Unhealthy)

## Hardware Required
- [Adafruit QT Py ESP32-S3](https://www.adafruit.com/product/5426)
- [Adafruit SEN66 Breakout](https://www.adafruit.com/product/5958)
- [STEMMA QT Cable](https://www.adafruit.com/product/4210)
- USB-C cable for power

## Wiring
Simply connect the QT Py and SEN66 breakout with a STEMMA QT cable - no soldering required!

## Installation

### Method 1: Using GitHub Package (Recommended)
1. Copy `sen66-device.yaml` to your ESPHome folder
2. Update the GitHub URL with your username
3. Create `secrets.yaml` with your WiFi credentials
4. Flash to your QT Py ESP32-S3

### Method 2: Direct Configuration
1. Copy the entire `sen66-base.yaml` content
2. Add your WiFi configuration
3. Flash directly to your device

## Configuration

### Multiple Sensors
Each sensor automatically gets a unique name using the MAC address suffix. After adoption in Home Assistant, rename them to their location:
- `air-quality-XXXXXX` → "Air Quality Kitchen"
- `air-quality-YYYYYY` → "Air Quality Bedroom"

### Calibration
Temperature and humidity offsets can be added if needed:
```yaml
sensor:
  - platform: sen6x
    temperature:
      filters:
        - offset: -2.0  # Adjust as needed
    humidity:
      filters:
        - offset: 5.0   # Adjust as needed
```

## Key Adaptations for QT Py ESP32-S3

### Correct I2C Pins
```yaml
i2c:
  sda: GPIO41  # NOT GPIO5!
  scl: GPIO40  # NOT GPIO6!
```
The QT Py's STEMMA QT connector uses GPIO41/40, not the default GPIO5/6.

### External Component Required
The SEN66 requires the external `sen6x` component from uELKO, not the built-in `sen5x`:
```yaml
external_components:
  - source:
      type: git
      url: https://github.com/uELKO/esphome_external_components
      ref: rework
    components: sen6x
```

### Framework
Uses ESP-IDF framework to avoid USB Serial compilation issues with Arduino framework on ESP32-S3.

## Troubleshooting

### No I2C devices found
- Check STEMMA QT cable is fully seated at both ends
- Verify using GPIO41/40 for I2C, not GPIO5/6
- Try the other STEMMA QT port on the QT Py

### CRC errors in logs
- Ensure using `sen6x` platform, not `sen5x`
- Check if device address is 0x6B instead of 0x69
- Try reducing I2C frequency to 50kHz

### Compilation errors
- Use ESP-IDF framework instead of Arduino
- Clean build files and retry

### Sensors showing "Unknown" in Home Assistant
- Wait 30-60 seconds for sensor initialization
- Check logs for communication errors
- Power cycle the device

## Home Assistant Integration

### Automations
Example automation for air quality alerts:
```yaml
automation:
  - alias: "Poor Air Quality Alert"
    trigger:
      - platform: state
        entity_id: sensor.air_quality_level
        to: "Unhealthy"
    action:
      - service: notify.mobile_app
        data:
          message: "Air quality is unhealthy!"
```

### Dashboard Card
```yaml
type: entities
entities:
  - entity: sensor.air_quality_level
  - entity: sensor.pm2_5
  - entity: sensor.co2
  - entity: sensor.temperature
  - entity: sensor.humidity
  - entity: sensor.voc_index
  - entity: sensor.nox_index
```

## LED Status Indicators
The QT Py's onboard NeoPixel indicates air quality:
- 🟢 **Green**: Good (PM2.5 < 12 µg/m³)
- 🟡 **Yellow**: Moderate (PM2.5 12-35.4 µg/m³)
- 🟠 **Orange**: Unhealthy for Sensitive (PM2.5 35.4-55.4 µg/m³)
- 🔴 **Red**: Unhealthy (PM2.5 55.4-150.4 µg/m³)
- 🟣 **Purple**: Very Unhealthy (PM2.5 > 150.4 µg/m³)

## Web Interface
Access the built-in web dashboard at:
- `http://air-quality-XXXXXX.local`
- `http://[DEVICE_IP_ADDRESS]`

## Acknowledgments
This project uses [uELKO's external components](https://github.com/uELKO/esphome_external_components) for proper SEN6x support. The configuration is adapted from their excellent [Air Quality Monitor project](https://github.com/uELKO/ESPHome-Air-Quality-Monitor).

## License
MIT License - See repository root for details