Follow the [Upload instructions](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload) to flash a new firmware version to your Sonoff module, just as before. Upgrading Sonoff-Tasmota is easier by OTA update.

### Upgrading via OTA

It is recommended to upgrade the firmware [over-the-air](https://en.wikipedia.org/wiki/Over-the-air_programming), without a serial connection, even while being connected to AC and operational.
This method is only available after the Sonoff-Tasmota firmware was flashed via serial connection once.

Build the firmware binary from source or download the latest build from [the releases section](https://github.com/arendst/Sonoff-Tasmota/releases). There are two ways to upgrade the firmware:

1. Use the "Upload Firmware" dialog on the Sonoff-Tasmota web interface.
2. Provide the binary on a web server and initiate the upgrade via the [`upgrade` command](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#management).