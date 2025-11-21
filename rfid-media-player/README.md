# RFID Media Player

Turn physical RFID cards into album/playlist triggers for a tactile music experience.

Based on [TheStockPot's RFID Jukebox project](https://github.com/TheStockPot/RFID-Jukebox).

## Features
- Scan RFID card → Play specific album
- Each card mapped to different music
- Perfect for kids or screen-free music control
- Wife-approved physical interface

## Setup
1. Connect PN532 to ESP32 via I2C
2. Flash configuration
3. Use Home Assistant to map card IDs to music actions

## Hardware Connections
- SDA → GPIO21
- SCL → GPIO22
- VCC → 3.3V
- GND → GND

## Acknowledgments
This project is adapted from TheStockPot's excellent [RFID Jukebox](https://github.com/TheStockPot/RFID-Jukebox) implementation.