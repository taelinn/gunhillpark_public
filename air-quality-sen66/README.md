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

### Step 1: Prepare Your Configuration
1. Copy `sen66-device.yaml` to your ESPHome folder
2. Update the GitHub URL with your username (or use mine: `taelinn`)
3. Create `secrets.yaml` from the template:
```yaml
wifi_ssid: "YOUR_WIFI_NAME"
wifi_password: "YOUR_WIFI_PASSWORD"
```

### Step 2: Initial Flash
The QT Py ESP32-S3 may show as "UNKNOWN device" on first flash. This is normal.

1. Connect QT Py via USB-C to your computer
2. In ESPHome, click **Install** → **Plug into this computer**
3. When you see "Platform unknown" error, click **CONTINUE ANYWAY**
4. Select your USB port and flash

The device configuration includes fallback settings that ensure the initial flash works even if the GitHub packages can't be loaded yet.

### Step 3: After First Flash
Once the device is online:
- It will automatically download the full configuration from GitHub
- The sensor will initialize (takes 30-60 seconds)
- All sensors will appear in Home Assistant

### Multiple Sensors
Each sensor automatically gets a unique name using the MAC address suffix:
- `air-quality-XXXXXX` → Rename to "Air Quality Kitchen"
- `air-quality-YYYYYY` → Rename to "Air Quality Bedroom"

## Configuration Details

### I2C Pin Mapping (Critical for QT Py)
The QT Py ESP32-S3's STEMMA QT connector uses different pins than standard:
```yaml
i2c:
  sda: GPIO41  # NOT GPIO5!
  scl: GPIO40  # NOT GPIO6!
```

### Required External Component
The SEN66 requires the external `sen6x` component from uELKO:
```yaml
external_components:
  - source:
      type: git
      url: https://github.com/uELKO/esphome_external_components
      ref: rework
    components: sen6x
```

## Troubleshooting

### "Platform unknown" or "UNKNOWN device" error
- Normal for first flash - click "Continue Anyway"
- The device YAML includes generic ESP32-S3 settings for initial compatibility

### No I2C devices found
- Check STEMMA QT cable is fully seated at both ends
- Verify using GPIO41/40 for I2C (not GPIO5/6)
- Green LED should be on the SEN66 breakout board

### CRC errors in logs
- Device address might be 0x6B instead of 0x69
- Try reducing I2C frequency to 50kHz if issues persist

### Sensors showing "Unknown" in Home Assistant
- Wait 30-60 seconds for sensor initialization
- Check ESPHome logs for communication errors
- Power cycle if needed

## Home Assistant Integration

### Example Automation
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
          message: "Air quality is unhealthy! PM2.5: {{ states('sensor.pm2_5') }}"
```

### Dashboard Card
```yaml
type: entities
title: Air Quality
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
The QT Py's onboard NeoPixel shows air quality at a glance:
- 🟢 **Green**: Good (PM2.5 < 12 µg/m³)
- 🟡 **Yellow**: Moderate (PM2.5 12-35.4 µg/m³)
- 🟠 **Orange**: Unhealthy for Sensitive (PM2.5 35.4-55.4 µg/m³)
- 🔴 **Red**: Unhealthy (PM2.5 55.4-150.4 µg/m³)
- 🟣 **Purple**: Very Unhealthy (PM2.5 > 150.4 µg/m³)

## Web Interface
Access the built-in dashboard at:
- `http://air-quality-XXXXXX.local`
- `http://[DEVICE_IP_ADDRESS]`

## Calibration
If sensors need adjustment:
```yaml
sensor:
  - platform: sen6x
    temperature:
      filters:
        - offset: -2.0  # Adjust if running warm
    humidity:
      filters:
        - offset: 5.0   # Adjust if needed
```

## Acknowledgments
This project uses [uELKO's external components](https://github.com/uELKO/esphome_external_components) for proper SEN6x support. The configuration is adapted from their excellent [Air Quality Monitor project](https://github.com/uELKO/ESPHome-Air-Quality-Monitor).

## License
MIT License - See repository root for details