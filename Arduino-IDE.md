How to setup and configure Arduino IDE for Tasmota compilation and upload.

## Download Arduino IDE
Download Arduino IDE from https://www.arduino.cc/en/Main/Software

I prefer a *dedicated standalone version* of the IDE allowing easy ESP8266 file manipulation and library management. This can be achieved by downloading the Arduino IDE ZIP file for non admin install.

## Install Arduino IDE
Unzip the installation file to a known folder.

IMPORTANT: Before executing *arduino.exe* add an empty folder called *portable* in the known folder.

## Download Tasmota
Download the latest Tasmota release Source code from https://github.com/arendst/Sonoff-Tasmota/releases and unzip to another known folder.

## Configure Arduino IDE
### Copy files
If not available copy from the Tasmota release Source code folder *arduino\version 2.3.0\tools\sdk\ld* file *eagle.flash.1m0.ld* to Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0\tools\sdk\ld*.

Replace in Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0* file *boards.txt* with the Tasmota Source code file *arduino\version 2.3.0\boards.txt*.

Copy all files from the Tasmota release Source code folder *lib* into your *sketchbook\libraries* folder.

Copy the Tasmota release Source code folder *sonoff* to your *sketchbook*.

### Change IDE parameters
Open Arduino IDE and select ``File`` - ``Preferences`` and add the following text for field *Additional Boards Manager URLs:* ``http://arduino.esp8266.com/stable/package_esp8266com_index.json`` and select *OK*.

Open ``Tools`` - ``Boards ...`` - ``Boards Manager ...`` and scroll down and click on *esp8266 by ESP8266 Community*. Click the *Install* button to download and install the ESP8266 board software. Select *Close*.

Select ``Tools`` and verify the following settings for **All Tasmota devices**:
```
Board: "Generic ESP8266 Module"
Flash Mode: "DOUT"
Flash Frequency: "40MHz"
Upload Using: "Serial"
CPU Frequency: "80MHz"
Flash Size: "1M (no SPIFFS)"
Debug Port: "Disabled"
Debug Level: "None"
Reset Method: "ck"
Upload Speed: "115200"
Port: Your COM port connected to sonoff
```

### Optional: Prepare for OTA upload
Tasmota release Source code provides scripts to be installed in the Arduino IDE and your webserver to copy the compiled binary to your webserver. This webserver can then provide the firmware via OTA to the device.

If not available install PHP on your webserver and copy the Tasmota release Source code folder *api* to the root of your webserver.

Replace in Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0* file *platform.txt* with the Tasmota Source code file *arduino\version 2.3.0\platform.txt*.

Copy from the Tasmota release Source code folder *arduino* file *espupload.py* to Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0\tools* and change in the script HOST_ADDR to point to your webserver ip address.

After restarting your Arduino IDE you now have an extra option for ``Tools`` - ``Upload Using: "OTA_upload"``.

## Compile Tasmota
Open Arduino IDE and select file *sonoff.ino* from your *sketchbook\sonoff* folder.

Compile Tasmota with ``Sketch`` - ``Verify/Compile``.

## Upload Tasmota to device
Arduino IDE uses the serial interface to upload the firmware to your device. On Windows these interfaces are named COM1, COM2 etc. On Linux these interfaces are called /dev/ttyUSB0, /dev/ttyUSB1 etc.

In the following commands I use COM5 as an example.

Before using Arduino IDE upload make sure you know to which serial interface name your device is connected to. 

### Put device in firmware upload mode
When performing a firmware upload do **not connect the device to AC** but use the power supply provided by your (FTDI type) serial interface.

Put the device in firmware upload mode by grounding pin GPIO00 while applying power.

Grounding pin GPIO00 can often be achieved by pressing button 1 on the Sonoff device or using a wire between GPIO00 and Gnd if the button is not available. Deviations may apply.

Connect the serial interface of your PC to the device while GPIO00 to Gnd.

### Perform serial upload
Make the correct serial interface selection in the Arduino IDE via ``Tools`` - ``Port: "COM5"``.

Upload the compiled firmware with ``Sketch`` - ``Upload``.

NOTE: For a proper device initialization after first firmware upload power down and power up the device.

## Optional: Upload Tasmota to OTA server
If a webserver is available you can upload the compiled firmware using optional scripts and prepare it for OTA download by any Tasmota device using the MQTT ``upgrade 1`` or ``upgrade 5.1.2`` command.

Make sure that ``Tools`` - ``Upload Using: "OTA_upload"`` is selected.

Upload the compiled firmware to the OTA server with ``Sketch`` - ``Upload``.


