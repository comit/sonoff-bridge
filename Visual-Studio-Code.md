How to setup and configure Visual Studio Code with PlatformIO for Tasmota compilation and upload.

## Download and Install Visual Studio Code
Download Visual Studio Code (VSC) from https://code.visualstudio.com/

### Install PlatformIO Extension
Install the _PlatformIO IDE_ extension in VSC.

Select ``View`` - ``Extensions`` and type PlatformIO in the search box.

Make sure to select the official PlatformIO.org *PlatformIO IDE* extension and select *Install*. Accept to install dependencies.

## Download Tasmota
Download the latest Tasmota release from https://github.com/arendst/Sonoff-Tasmota/releases and unzip to a known folder.

### Copy files
Copy all files from the Tasmota release Source code into your VSC working folder.

## Compile Tasmota
Start VSC and select ``File`` - ``Open Folder...` to point to the working folder.

**Note:** Press `Ctrl` + `Shift` + `P` and type `PlatformIO` to see all options.

Select the desired firmware by editing file _platformio.ini_ as needed.

Easy compilation can be performed from the icons at the bottom of the VSC screen. 

## Upload Tasmota

Enable desired options in _patformio.ini_ for serial upload like:
```
; *** Upload Serial reset method for Wemos and NodeMCU
upload_port = COM5
;upload_speed = 512000
upload_speed = 115200
;upload_resetmethod = nodemcu
``

Enable desired options in _patformio.ini_ for upload to your local OTA server like:
```
; *** Upload file to OTA server using HTTP
upload_port = domus1:80/api/upload-arduino.php
extra_scripts = pio/http-uploader.py
```

Easy compilation and upload can be performed from the icons at the bottom of the VSC screen. 

Use `Ctrl` + `Alt` + `U` to upload (will build if needed).
