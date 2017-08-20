How to setup and configure Arduino IDE for Tasmota compilation and upload.

## Download Arduino IDE
Download Arduino IDE from https://www.arduino.cc/en/Main/Software

I prefer a *dedicated standalone version* of the IDE allowing easy ESP8266 file manipulation and library management. This can be achieved by downloading the Arduino IDE ZIP file for non admin install.

## Install Arduino IDE
Unzip the installation file to a known folder.

IMPORTANT: Before executing *arduino.exe* add an empty folder called *portable* in the known folder.

## Download Tasmota
Download the latest Tasmota release Source code from https://github.com/arendst/Sonoff-Tasmota/releases and unzip to another known folder.

## Configure Arduino ide
If not available copy from the Tasmota release Source code folder *arduino\version 2.3.0\tools\sdk\ld\* file *eagle.flash.1m0.ld* to Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0\tools\sdk\ld*

Replace in Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0\* file *boards.txt* with the Tasmota Source code file *arduino\version 2.3.0\boards.txt*.

Copy all files from the Tasmota release Source code folder *lib* into your *sketchbook\libraries* folder.

Copy the Tasmota release Source code folder *sonoff* to your *sketchbook*

Open Arduino IDE and select ``File`` - ``Preferences`` and add the following text for field *Additional Boards Manager URLs:* ``http://arduino.esp8266.com/stable/package_esp8266com_index.json`` and select OK.

In the IDE open ``Tools`` - ``Boards ...`` - ``Boards Manager ...`` and scroll down and click on *esp8266 by ESP8266 Community*. Click the install button to download and *Install* the Tasmota needed ESP8266 board software.

Open Arduino IDE and select ``Tools`` - ``Boards: "Generic ESP8266 Module"``.



## Compile Tasmota
Open Arduino IDE

## Upload Tasmota