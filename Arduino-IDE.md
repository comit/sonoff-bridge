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
If not available copy from the Tasmota repository file *arduino\version 2.3.0\tools\sdk\ld\eagle.flash.1m0.ld* to Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0\tools\sdk\ld*

Replace file Arduino IDE folder *portable\packages\esp8266\hardware\esp8266\2.3.0\boards.txt* with the Tasmota repository file *arduino\version 2.3.0\boards.txt*.


## Compile Tasmota


## Upload Tasmota