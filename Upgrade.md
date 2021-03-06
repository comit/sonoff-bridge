For first-time flashers, please follow the [Upload instructions](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload) to initially flash the Sonoff-Tasmota firmware onto your Sonoff module.

## Upgrading via OTA

It is recommended (and more convenient) to upgrade the firmware of a Sonoff module [over-the-air](https://en.wikipedia.org/wiki/Over-the-air_programming), without a serial connection, while being connected to AC and operational.
This method is only available after the Sonoff-Tasmota firmware was flashed via serial connection once.

### OTA Upgrade Methods

**Attention:** Because of limited flash memory it might be needed to flash a *minimal* firmware version before flashing the actual *non-minimal* new firmware update. See below or more details.

There are a few ways to upgrade the firmware:

1. Use the "Upgrade by file upload" dialog on the Sonoff-Tasmota web interface to flash an available firmware binary file (e.g. `firmware.bin`)
2. Use the "Upgrade by web server" dialog on the Sonoff-Tasmota web interface to flash a firmware binary file provided on a web server
3. Initiate the upgrade via the serial/MQTT/web command [`upgrade`](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#management)

*Bonus for openHAB users:* Implement an [automation rule](https://github.com/arendst/Sonoff-Tasmota/wiki/openHAB#maintenance-actions) to upgrade your Sonoffs from within openHAB.

### Firmware Binary Sources

Build the firmware binary from source or download the latest build.

The latest build can be found in the [GitHub releases section](https://github.com/arendst/Sonoff-Tasmota/releases). 
A secondary hosting of the files is provided by [@smadds](https://github.com/arendst/Sonoff-Tasmota/issues/19) because GitHub will only serve files over HTTPS, which is not supported by the Sonoff-Tasmota firmware.

A full list of available builds for the latest release can be found in [the releases section](https://github.com/arendst/Sonoff-Tasmota/releases). They are all provided in the format: `http://sonoff.maddox.co.uk/tasmota/BUILDNAME.bin`

The URLs to the latest builds are (starting from 5.1.4): 

* http://sonoff.maddox.co.uk/tasmota/sonoff.bin (standard build)
* http://sonoff.maddox.co.uk/tasmota/sonoff.ino.bin (identical, for compatibility)
* http://sonoff.maddox.co.uk/tasmota/sonoff-minimal.bin (see below)
* http://sonoff.maddox.co.uk/tasmota/sonoff-ds18x20.bin (for multiple DS18x20 sensors)

**Bonus - Update Notification:** [@smadds](https://github.com/arendst/Sonoff-Tasmota/issues/19) also publishes the latest released version number to a publicly available MQTT server which may be subscribed to at `sonoff.maddox.co.uk:1883` with the topic `sonoff-version`. No authentication needed.

### Functionality vs Firmware size vs OTA

As more functionality is being added to the firmware at some time it reaches the point where OTA or web page upload will fail. Both upgrade features rely on the fact that there is enough free flash memory available to load both the current and the new firmware. With a 1MB flash as available on the Sonoffs and the standard linker script providing 950kB code space a firmware file size of as much as 475kB allows for an easy upgrade. 

Starting with **version 5.x** Sonoff-Tasmota uses an updated linker script, extending the code space by 70kB, allowing a firmware file size of as much as 510kB.

Larger firmware files can only be loaded when the current flash usage is first reduced to accommodate more free flash to load the new firmware. Hence you'll need to perform a two step process:
1. Upload firmware with `#define USE_MINIMAL` **enabled** which will have a smaller footprint
2. Upload the final firmware with `#define USE_MINIMAL` **disabled** with the features you want to use.

## Migration path

Until now several versions of my Sonoff software have been released starting with the C version Sonoff-MQTT-OTA followed by Sonoff-MQTT-OTA-Arduino and Sonoff-Tasmota.

Migrating from one version to the next versions is mostly painless as the settings are saved in the same location in flash and newer settings are appended.

As said, mostly painless. There are some deviations to this rule as I rearranged the flash. In the next list you'll find an overview of supported migrations paths.

1. No migration from **Sonoff-MQTT-OTA** to **Sonoff-MQTT-OTA-Arduino** or **Sonoff-Tasmota**.<br/>The settings flash lay-out and OTA image locations are different from the Arduino versions
2. Easy migration from **Sonoff-MQTT-OTA-Arduino 1.0.11** to **Sonoff-Tasmota 3.9.x**.<br/>After installing Sonoff-Tasmota for the first time some settings need to be adjusted via web configuration or MQTT commands.
3. Easy migration from **Sonoff-MQTT-OTA-Arduino 3.1.0** to **Sonoff-Tasmota 4.x**.<br/>After installing Sonoff-Tasmota for the first time some settings need to be adjusted via web configuration or MQTT commands.
4. Easy migration from **Sonoff-Tasmota 4.x** to **Sonoff-Tasmota 5.x**.<br/>As a safeguard perform a Backup Configuration before installing the new version. If settings are lost after the upgrade perform a Restore Configuration.

So to migrate from **Sonoff-MQTT-OTA-Arduino versions before 3.1.0** to **Sonoff-Tasmota 5.x** you will need to take three steps:

1. Migrate to **Sonoff-Tasmota 3.9.x**
2. Migrate to **Sonoff-Tasmota 4.x**
3. Migrate to **Sonoff-Tasmota 5.x**
