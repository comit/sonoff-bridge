### 20170714 - Tasmota needs compile Flash Mode option DOUT on all devices

An increasing number of devices is using the ESP8285. As this chip only supports a subset of hardware connections to it's inbuilt 1MB flash the firmware needs to be compiled with Flash Mode option set for DOUT.

This is documented in the [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload) and the platformio.ini file in the repository.

As this compile option works for the ESP8266 "legacy" chip too I only provide released firmware compiled with Flash Mode DOUT which runs fine on all released hardware like Sonoff, Wemos etc. See also [#598](https://github.com/arendst/Sonoff-Tasmota/issues/598).

Indications of selecting the wrong Flash Mode compile option (like DIO or QIO) on ESP8285 is a dead device while uploading just went fine [#683](https://github.com/arendst/Sonoff-Tasmota/issues/683).

### 20170619 - Configuration settings save locations in flash

Configuration settings are saved in flash. Over time different areas of flash have been used as can be seen in the picture below.

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/memmap1.jpg" />

To reduce flash wear I started to use a number of rotating flash pages with version 5.2. To still be able to use maximum program size during OTA or webpage upgrades I copy the latest config to the EEPROM area just before the upgrade starts.

### 20170425 - Tasmota version 5.x and up need linker script 1M with No SPIFFS

Starting with version 5.x a new linker script is made available allowing for 32k more code space.

See [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisite) how to implement it in your Arduino or platformIO IDE.

### 20170403 - Tasmota supported libraries in repository

All Tasmota external libraries are available in the lib folder of the repository.

To save as much code space as possible only these library versions are supported by Tasmota. See [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisite) for individual library installation.

### 20170124 - Sonoff-Tasmota spin-off from Sonoff-MQTT-OTA-Arduino

The goal of Sonoff-Tasmota (Tasmota for short) is to provide one pre-compiled MQTT enabled, "Over the Air" firmware with support for as much (iTead Sonoff) devices as possible without the need to edit a user_config.h file. Configuration settings can be changed online without re-compilation.

All this within the tight code space that 1MB flash allows. 