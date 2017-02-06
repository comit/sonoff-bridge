## PlatformIO

1. Load the Sonoff-Tasmota base folder, including [platformio.ini](https://github.com/arendst/Sonoff-Tasmota/blob/master/platformio.ini), in [PlatformIO](https://github.com/platformio).
2. Connect the Sonoff module in Flash Mode (see Hardware section)
2. Select "Upload"
3. Finished

## Arduino IDE

Make sure you configured the IDE as described in [**Prerequisite**](Prerequisite)!

Load the file `sonoff.ino` into the IDE.

In the Arduino IDE for sonoff select from `Tools Board Generic ESP8266 Module` and set the following options:

- Upload Using: Serial
- Flash Mode: DIO
- Flash Frequency: 40MHz
- CPU Frequency: 80MHz
- Flash Size: 1M (64K SPIFFS)
- Debug Port: Disabled
- Debug Level: None
- Reset Method: ck
- Upload Speed: 115200
- Port: Your COM port connected to sonoff

Verify and/or compile the project and upload to your sonoff using the serial connection established above.

### Uploading via OTA

This method is only available after flashing Sonoff-Tasmota once.

Verify and upload an OTA image to your web server with option `Upload Using: OTA_upload`.

You may also use sonoffs web server and upload the file directly.

Enable debug messages with command `seriallog 3`.