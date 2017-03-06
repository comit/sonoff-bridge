## PlatformIO

1. Download the latest [Sonoff-Tasmota "Source code (zip)"](https://github.com/arendst/Sonoff-Tasmota/releases) and extract it
2. Load the Sonoff-Tasmota base folder, including [platformio.ini](https://github.com/arendst/Sonoff-Tasmota/blob/master/platformio.ini), in [PlatformIO](https://github.com/platformio).
3. Connect the Sonoff module in Flash Mode (see Hardware section)
4. Select "Upload".

Continue at ["First Steps"](https://github.com/arendst/Sonoff-Tasmota/wiki/Initital-Configuration) and be sure to check out the instructions to [connect additional sensors](https://github.com/arendst/Sonoff-Tasmota/wiki/Sensor-Configuration)

## Arduino IDE

Make sure you configured the IDE as described in [**Prerequisite**](Prerequisite)!

Load the file `sonoff.ino` into the IDE.

In the Arduino IDE for sonoff select from `Tools Board Generic ESP8266 Module` ( `Tools Board Generic ESP8285 Module` for CH4 version) and set the following options:

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

## Hints

Before compiling consider modification of `STA_SSID1` and `STA_PASS1` values inside of `user_config.h` to match your WiFi SSID and WiFi password.

    #define STA_SSID1              "indebuurt1"      // [Ssid1] Wifi SSID 
    #define STA_PASS1              "VnsqrtnrsddbrN"  // [Password1] Wifi password 

This step simplifies further [first steps](Initital-Configuration), as Sonoff will automatically recognize and join your WiFi.
