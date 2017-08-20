How to setup and configure Esptool for Tasmota upload.

## Download Esptool
If you do not have an installed copy of Python 2.x or 3.x download and install it from https://www.python.org/.

Download Esptool Source code from https://github.com/espressif/esptool/releases to a known folder.

## Install Esptool
Go to the known folder and install Esptool with command ``python setup.py install``.

## Download Tasmota
Download the latest Tasmota release firmware file *sonoff.bin* from https://github.com/arendst/Sonoff-Tasmota/releases to a known folder.

## Upload Tasmota
Esptool uses the serial interface to communicate with your device. On Windows these interfaces are named COM1, COM2 etc. On Linux these interfaces are called /dev/ttyUSB0, /dev/ttyUSB1 etc.

Before using Esptool make sure you know to which serial interface name your device is connected to.

In the following commands I use COM5 as an example.

### Put device in firmware upload mode
When performing a firmware upload do **not connect the device to AC** but use the power supply provided by your (FTDI type) serial interface.

Put the device in firmware upload mode by grounding pin GPIO00 while applying power.

Grounding pin GPIO00 can often be achieved by pressing button 1 on the Sonoff device or using a wire between GPIO00 and Gnd if the button is not available. Deviations may apply.

Connect the serial interface of your PC to the device while GPIO00 to Gnd.

### Optional: Backup firmware
Ensure the device is in firmware upload mode.

Backup the current firmware with the following command:
```
esptool.py --port COM5 read_flash 0x00000 0x100000 image1M.bin
```
NOTE: When the command completes the device is out of firmware upload mode!

### Optional: Erase firmware
Ensure the device is in firmware upload mode.

Erase the complete flash memory holding the firmware with the following command:
```
esptool.py --port COM5 erase_flash
```
NOTE1: When the command completes the device is out of firmware upload mode!

NOTE2: It only takes a few seconds to erase 1M of flash.

### Upload firmware
Ensure the device is in firmware upload mode.

Load the downloaded Tasmota firmware file *sonoff.bin* with the following command:
```
esptool.py --port COM5 write_flash -fs 1MB -fm dout 0x0 sonoff.bin
```
NOTE1: When the command completes the device is out of firmware upload mode!

NOTE2: For a proper device initialisation after first firmware upload power down and power up the device.

