For first-time flashers, please follow the [Upload instructions](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload) to flash the latest firmware version to your Sonoff module. Upgrading existing Sonoff-Tasmota (at least since v3.9.12) is easier by OTA update.

### Upgrading via OTA

It is recommended to upgrade the firmware [over-the-air](https://en.wikipedia.org/wiki/Over-the-air_programming), without a serial connection, while being connected to AC and operational.
This method is only available after the Sonoff-Tasmota firmware was flashed via serial connection once.

Build the firmware binary from source or download the latest build from [the releases section](https://github.com/arendst/Sonoff-Tasmota/releases). There are a few ways to upgrade the firmware:

1. Use the "Upload Firmware" dialog on the Sonoff-Tasmota Web Interface to flash the downloaded or built `firmware.bin`.
2. [@smadds](https://github.com/arendst/Sonoff-Tasmota/issues/19) publicly provides an automatic build of the latest firmware on his webserver. Enter this URL on the Sonoff-Tasmota web interface: http://sonoff.maddox.co.uk/tasmota/sonoff.ino.bin <br>*(Github only provides SSL-encrypted links and the Sonoff isn't capable of connecting via https protocol.)*
3. Provide the binary on a web server and initiate the upgrade via the [`upgrade` command](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#management).
4. If you're using [openHAB2](http://www.openhab.org/), you can use an automation rule to upgrade your Sonoffs from within your home automation: [openHAB Maintenance](https://github.com/arendst/Sonoff-Tasmota/wiki/openHAB#maintenance-actions)

### Migration path

Until now several versions of my Sonoff software have been released starting with the C version Sonoff-MQTT-OTA followed by Sonoff-MQTT-OTA-Arduino and Sonoff-Tasmota.

Migrating from earlier versions to newer versions is mostly painless as the settings are saved in the same location in flash while newer settings are appended.

As said, mostly painless. There are some deviations to this rule. In the next list you'll find an overview of supported migrations paths.

1) No migration from Sonoff-MQTT-OTA to Sonoff-MQTT-OTA-Arduino or Sonoff-Tasmota
2) Easy migration from Sonoff-MQTT-OTA-Arduino 1.0.11 to Sonoff-Tasmota 3.9.x
3) Easy migration from SOnoff-MQTT-OTA-Arduino 3.x to Sonoff-Tasmota 4.x

So to migrate from Sonoff-MQTT-OTA-Arduino versions below 3.x to Sonoff-Tasmota 4.x you will need to take two steps:
1) Migrate to Sonoff-Tasmota 3.9.x
2) Migrate to SOnoff-Tasmota 4.x