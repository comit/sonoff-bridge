### 20180110 - 5.11.1a - Automagic OTA functionality

Tasmota is build to install on devices with 1MB usable flash. To easily use Over The Air (OTA) firmware updates only half of this flash space can be used by Tasmota as the other half is needed to store the OTA firmware image before it can be installed.

To keep Tasmota this small I had to reduce redundancy as much as possible. I also had to tweak some libraries by removing default features and disable some features in the user_config.h.

This worked kind of fine until version 2.4.0 of the [ESP8266/Arduino board manager](https://github.com/esp8266/Arduino) came along. This version needs an extra 22k for it's libraries and functionality. To provide easy OTA updates I had to tweak Tasmota even further to the point that major features like Emulation almost had to be disabled by default.

Luckily the ESP8266/Arduino board manager software adjusts it's free flash space based on the current program space. This means that a small program has more OTA flash space to use than a large program. I used this feature by providing the sonoff-minimal.bin firmware image containing only MQTT and a webserver which allows for larger Tasmota images by using a two step OTA approach.

- First OTA upload the sonoff-minimal.bin image in the small free space area
- Then OTA upload the final sonoff.bin image in the larger free space area

With the pre-release version of **Tasmota development branch v5.11.1a** this process is now automated if used with an external OTA server.

Tasmota now tries to load the requested image and if it notices that the image won't fit it will load the minimal version first which in turn will load the requested final image.

#### Under the hood

Updated libraries
- TasmotaSerial-1.0.1

### 20180107 - 5.11.1 - Release 

The use of a different language from English has been revisited due to incompatible JSON messages between Tasmota and external tools (#1473). This has resulted in major changes to the language files which now only translate the Web GUI and Logging messages. All JSON messages will now be in English. 

The response from a HTPP request will now be in plain JSON only. This allows for better integration with external tools. As a result the special Energy status message has been abandoned and is now provided as a Sensor status message. Useres may have to update their tools monitoring Energy values. 

The ''Color2'', ''Color3'' and ''Color4'' commands have been renamed to respectively ''Color3'', ''Color4'' and ''Color5'' to make room for the new ''Color2'' command.

#### Under the hood

The introduction of Device function pointers should make future integration of new devices easier. 

New libraries have been added:
- Adafruit_BME680-1.0.5
- Adafruit_Sensor-1.0.2.02
- TasmotaSerial-1.0.0
- TSL2561-Arduino-Library
