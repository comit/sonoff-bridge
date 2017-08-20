How to setup and configure Visual Studio Code for Tasmota compilation and upload.

## Download Visual Studio Code
Download Visual Studio Code from https://code.visualstudio.com/

## Install Visual Studio Code
Install Visual Studio Code after downloading

## Download Tasmota
Download the latest Tasmota release from https://github.com/arendst/Sonoff-Tasmota/releases and unzip to a known folder.

## Configure Visual Studio Code
Install the PlatformIO IDE extension in Virtual Studio Code.

Select ``View`` - ``Extensions`` and type PlatformIO in the search box.

Be careful to select the official PlatformIO.org *PlatformIO IDE* extension and select *Install*. Accept to install dependencies.

0. Open the folder where you checked out sonoff-tasmota in VSCode.
0. Adapt `user_config.h` to your wishes.
0. Use `Ctrl` + `Alt` + `U` to upload (will build if needed).

**Note:** Press `Ctrl` + `Shift` + `P` and type `PlatformIO` to see all options.

### Copy files
Copy all files from the Tasmota release Source code into your Virtual Studio base folder.

## Compile Tasmota
Open the Visual Studio Code base folder and select the Sonoff-Tasmota folder.

## Upload Tasmota