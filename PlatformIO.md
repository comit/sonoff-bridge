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
Select ``Compile`` from the menu.

## Upload Tasmota
PlatformIO uses the serial interface to upload the firmware to your device. On Windows these interfaces are named COM1, COM2 etc. On Linux these interfaces are called /dev/ttyUSB0, /dev/ttyUSB1 etc.

### Put device in firmware upload mode
When performing a firmware upload do **not connect the device to AC** but use the power supply provided by your (FTDI type) serial interface.

Put the device in firmware upload mode by grounding pin GPIO00 while applying power.

Grounding pin GPIO00 can often be achieved by pressing button 1 on the Sonoff device or using a wire between GPIO00 and Gnd if the button is not available. Deviations may apply.

Connect the serial interface of your PC to the device while GPIO00 to Gnd.

### Perform serial upload
Select ``Upload`` from the menu.

NOTE: For a proper device initialization after first firmware upload power down and power up the device.
