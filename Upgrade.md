For first-time flashers, please follow the [Upload instructions](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload) to flash the latest firmware version to your Sonoff module. Upgrading existing Sonoff-Tasmota (at least since v3.9.12) is easier by OTA update.

### Upgrading via OTA

It is recommended to upgrade the firmware [over-the-air](https://en.wikipedia.org/wiki/Over-the-air_programming), without a serial connection, while being connected to AC and operational.
This method is only available after the Sonoff-Tasmota firmware was flashed via serial connection once.

Build the firmware binary from source or download the latest build from [the releases section](https://github.com/arendst/Sonoff-Tasmota/releases). There are two ways to upgrade the firmware:

1. Use the "Upload Firmware" dialog on the Sonoff-Tasmota web Interface after you download the latest firmware.bin.
2. Use the Service of @smadds, as he automatically uploads the binaries of the [firmware on his server](https://github.com/arendst/Sonoff-Tasmota/issues/19). Github only provides SSL-encrypted links and the Sonoff isn't capable of connecting via https protocol. You can use these URLs for automatic update:
 - http://sonoff.maddox.co.uk/tasmota/sonoff.ino.bin
 - http://sonoff.maddox.co.uk/tasmota/sonoff-nodemcu.ino.bin
 - http://sonoff.maddox.co.uk/tasmota/sonoff-8285.ino.bin
3. Provide the binary on a web server and initiate the upgrade via the [`upgrade` command](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#management).
4. If you're using [openHAB2](http://www.openhab.org/), you can use some rules for updating the Sonoff in openHAB-environment: [openHAB Maintenance](https://github.com/arendst/Sonoff-Tasmota/wiki/openHAB#maintenance-actions)