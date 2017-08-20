How to setup and configure Visual Studio for Tasmota compilation and upload.

## Download Visual Studio
Download Visual Studio from https://code.visualstudio.com/

## Install Visual Studio
Install Visual Studio after downloading

## Download Tasmota
Download the latest Tasmota release from https://github.com/arendst/Sonoff-Tasmota/releases and unzip to a known folder.

## Configure Visual Studio
Configure Visual Studio for Arduino with ESP8266 support.

### Using PlatformIO

0. Install the PlatformIO IDE extension in VS Code, then use instructions for PlatformIO on the device specific pages.
0. Open the folder where you checked out sonoff-tasmota in VSCode.
0. Adapt `user_config.h` to your wishes.
0. Use `Ctrl` + `Alt` + `U` to upload (will build if needed).

**Note:** Press `Ctrl` + `Shift` + `P` and type `PlatformIO` to see all options.

### Using Arduino
How? You tell me.

As far as I can tell the Visual Studio Arduino Extension needs access to an installed Arduino IDE. Make sure you use a freshly installed Arduino IDE as the extension initially will remove any reference to ESP8266 (at least that's what happenend in my case). 

## Compile Tasmota


## Upload Tasmota