![SL-OmniTrack Banner](Readme%20Resources/Banner_1.png)

## Overview

**SL-OmniTrack** - "SL" stands for "Silicon Labs" and "OmniTrack" is my flagship project under Silicon Labs. It has 2 modules -
1. **Avionics bay** - It has microcontroller like ESP32-S3 and sensors like IMU, Barometer, GPS, MicroSD module, SX1278 and power system with 1*18650 and MT3608.
 
  **Function** - All sensors log all their data in SD card and send those data in time-wise packets to **Ground Station**.

2. **Ground Station** - It mainly has ESP32-S3, SX1278, USB cable to connect it to laptop/pc.

  **Function** - It receive those data sent from *Avionics Bay* and send all data to a native software (Yet to build) via usb cable.

 ## Freedom

 - My main goal is to give you freedom of using this product however you like. Enclousure design gives freedom to flash your own code, change sensors etc.

 ## Planned Bus Layout

 **Avionics Bay:**

| Component | Interface | Power Source |
|---|---|---|
| MPU9250/MPU6500 (IMU) | I2C | ESP32-S3 onboard 3.3V regulator |
| BMP280 (barometer) | I2C | ESP32-S3 onboard 3.3V regulator |
| SX1278 LoRa module | SPI | ESP32-S3 onboard 3.3V regulator |
| NEO-6M GPS | UART | 5V rail (via MT3608 boost) |
| MicroSD module | SPI | 5V rail (via MT3608 boost) |
| ESP32-S3 | — | 5V rail (via MT3608 boost) |

**Ground Station:**

| Component | Interface | Power Source |
|---|---|---|
| SX1278 LoRa module | SPI | ESP32-S3 onboard 3.3V regulator |
| 1.3" I2C OLED | I2C | ESP32-S3 onboard 3.3V regulator |
| ESP32-S3 | — | USB (laptop) |