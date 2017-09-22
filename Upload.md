## PlatformIO

1. Download the latest [Sonoff-Tasmota "Source code (zip)"](https://github.com/arendst/Sonoff-Tasmota/releases) and extract it - you may also *git clone* the repository
2. Load the Sonoff-Tasmota base folder in [PlatformIO](https://github.com/platformio)
3. Connect the Sonoff module in Flash Mode (see [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) section and device specific articles)
4. Select a firmware variant to flash onto your module by uncommenting one of the `env_default` lines in [platformio.ini](https://github.com/arendst/Sonoff-Tasmota/blob/master/platformio.ini), see below for variant details
5. Open `user_config.h` and configure your WiFi settings and optionally your MQTT, Syslog, WebServer, NTP, etc. settings
6. Select "Upload" from the menu to flash the firmware 
7. After successful transfer of the firmware disconnect the module

Continue at ["First Steps"](https://github.com/arendst/Sonoff-Tasmota/wiki/Initial-Configuration) and be sure to check out the instructions to [connect additional sensors](https://github.com/arendst/Sonoff-Tasmota/wiki/Sensor-Configuration).

#### Firmware Variants

The default environment configuration generates multiple firmware variants. To build and/or flash exactly one of these, uncomment one of the `env_default` lines in [platformio.ini](https://github.com/arendst/Sonoff-Tasmota/blob/master/platformio.ini).

* `sonoff.bin` - the default firmware for all devices
* `sonoff-minimal.bin` - is interim firmware to be used when the above firmware images become too big to fit as OTA or web upload; installing this one first and THEN uploading the desired sonoff.bin allows for future firmware size growth over the OTA file limit of 1/2 flash size.
* `sonoff-ds18x20.bin` - is a version of sonoff.bin with the USE_DS18X20 define enabled and a larger MQTT buffer size to be used by people having more than 4 ds18x20 sensors connected.

## Arduino IDE

1. Make sure you configured the IDE as described in [Prerequisite](Prerequisite)!
2. Load the file `sonoff.ino` into the IDE.
3. In the Arduino IDE for sonoff select from `Tools Board Generic ESP8266 Module` ( `Tools Board Generic ESP8285 Module` for CH4 version) and set the following options:
- Upload Using: Serial
- Flash Mode: DOUT
- Flash Frequency: 40MHz
- CPU Frequency: 80MHz
- Flash Size: 1M (64K SPIFFS) ⚠️️**If Version 5.x.x -> Flash Size: "1M (no SPIFFS)"**⚠️️
- Debug Port: Disabled
- Debug Level: None
- Reset Method: ck
- Upload Speed: 115200
- Port: Your COM port connected to sonoff
4. Open `user_config.h` and configure your WiFi settings and optionally your MQTT, Syslog, WebServer, NTP, etc. settings
5. Verify and/or compile the project and upload to your sonoff using the serial connection established above.

Continue at ["First Steps"](https://github.com/arendst/Sonoff-Tasmota/wiki/Initial-Configuration) and be sure to check out the instructions to [connect additional sensors](https://github.com/arendst/Sonoff-Tasmota/wiki/Sensor-Configuration).

## Sonoff factory OTA Mechanism (experimental)

There's now also a script with which you can flash your freshly bought unmodified Sonoff device via the original internal OTA upgrade mechanism provided by the Itead firmware. No need to open and solder header pins, no need for a serial programmer.

For further details see: https://github.com/mirko/SonOTA

There is also a fork here that steps through the process with pre-compiled binary files: https://github.com/sillyfrog/SonOTA

----

## Device Configuration

Before compiling consider modification of `STA_SSID1` and `STA_PASS1` values inside of `user_config.h` to match your WiFi SSID and WiFi password.

    #define STA_SSID1              "indebuurt1"      // [Ssid1] Wifi SSID 
    #define STA_PASS1              "VnsqrtnrsddbrN"  // [Password1] Wifi password 

This operation simplifies further [first steps](https://github.com/arendst/Sonoff-Tasmota/wiki/Initial-Configuration), as Sonoff will automatically recognize and join your WiFi.

Do not be tempted to use other board types within the Arduino IDE (e.g. if programming Wemos etc.) if you want to use Tasmota's OTA updating, as this can cause failure and require serial port reprogramming. Always use the Generic ESP board as stated above.

If your new configuration does not seem to be applied after flashing then you can do any of the following to fix it:
* Change the CFG_HOLDER value in user_config.h and re-flash
* Hold down the button on your device for at least 4 seconds to initiate a RESET command
* Reset the device via the Web Console