# ESPHome Configurations

A collection of ESPHome configurations for various smart home sensors and devices. All configurations are designed to work with Home Assistant.

## Projects

### 🎵 [RFID Media Player](./rfid-media-player/)
Physical media experience for digital music using RFID cards as "cartridges" for albums.
- **Hardware**: ESP32 + PN532 NFC/RFID reader
- **Features**: Scan RFID cards to play specific albums/playlists through Home Assistant
- **Use Case**: Kid-friendly music player, vinyl-like experience for digital music

### 🌡️ [Air Quality Sensor (SEN66)](./air-quality-sen66/)
Comprehensive air quality monitoring with PM, CO2, temperature, humidity, VOC, and NOx.
- **Hardware**: Adafruit QT Py ESP32-S3 + Sensirion SEN66
- **Features**: PM1.0/2.5/4.0/10, CO2, temp/humidity, VOC/NOx, status LED, web interface
- **Use Case**: Indoor air quality monitoring, automation triggers

### 📡 [Bluetooth Proxy](./bluetooth-proxy/)
Extend Home Assistant's Bluetooth reach throughout your home.
- **Hardware**: Various ESP32 boards
- **Features**: Active/passive BLE scanning, extends HA Bluetooth coverage
- **Use Case**: Track BLE devices, connect to Bluetooth sensors in remote locations

## Quick Start

### Using Package Import (Recommended)

1. Copy the appropriate device YAML to your ESPHome folder
2. Update with your GitHub username:
```yaml
packages:
  device_config:
    url: https://github.com/YOUR_USERNAME/esphome-configs
    ref: main
    files: [path/to/base.yaml]
```
3. Create `secrets.yaml` from the template
4. Install to your ESP device

### Manual Installation

1. Copy the complete YAML configuration
2. Create `secrets.yaml` with your WiFi credentials
3. Flash to your ESP device

## Common Features

All devices include:
- Automatic unique naming with MAC suffix
- WiFi fallback AP mode
- Web server interface
- Home Assistant API integration
- OTA updates

## Secrets Template

Copy `secrets.yaml.example` to `secrets.yaml` and update:
```yaml
wifi_ssid: "YOUR_WIFI_NAME"
wifi_password: "YOUR_WIFI_PASSWORD"
```

## Hardware Links

### RFID Media Player
- ESP32 Development Board
- [PN532 NFC/RFID Module](https://www.adafruit.com/product/364)
- RFID cards/tags

### Air Quality Sensor
- [Adafruit QT Py ESP32-S3](https://www.adafruit.com/product/5426)
- [Sensirion SEN66 Sensor](https://www.adafruit.com/product/5958)
- STEMMA QT cable

### Bluetooth Proxy
- Any ESP32 board (ESP32-C3, ESP32-S3, etc.)
- No additional hardware required

## Contributing

Feel free to open issues or PRs for improvements. If you've adapted these configs for different hardware, I'd love to see your modifications!

## Credits

- SEN66 support via [uELKO's external components](https://github.com/uELKO/esphome_external_components)
- ESPHome community for inspiration and examples

## License

MIT License - See [LICENSE](LICENSE) file for details
