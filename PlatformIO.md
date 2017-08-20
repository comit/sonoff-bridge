How to setup and configure PlatformIO for Tasmota compilation and upload.

## Download PlatformIO
Download PlatformIO from http://platformio.org/

## Install PlatformIO
Install PlatformIO to a known folder.

## Download Tasmota
Download the latest Tasmota release Source code from https://github.com/arendst/Sonoff-Tasmota/releases and unzip to another known folder.

## Configure PlatformIO
### Copy files
Copy all files from the Tasmota release Source code into your platformIO base folder.

### Change IDE parameters
The default environment configuration generates multiple firmware variants. To build and/or flash exactly one of these, uncomment one of the *env_default* lines in file *platformio.ini*.
- *sonoff.bin* - the default firmware for all devices
- *sonoff-minimal.bin* - is interim firmware to be used when the above firmware images become too big to fit as OTA or web upload; installing this one first and THEN uploading the desired *sonoff.bin* allows for future firmware size growth over the OTA file limit of 1/2 flash size.
- *sonoff-ds18x20.bin* - is a version of *sonoff.bin* with the USE_DS18X20 define enabled and a larger MQTT buffer size to be used by people having more than 4 ds18x20 sensors connected.

## Compile Tasmota


## Upload Tasmota