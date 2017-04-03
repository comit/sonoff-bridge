## Needed Hardware

* One of the [supported ESP8266 modules](https://github.com/arendst/Sonoff-Tasmota/blob/master/README.md)
* 3.3V FTDI USB-to-Serial Converter/Programmer ([example](https://www.sparkfun.com/products/9873))
* Soldering iron and header pins

## Needed Software

Use either of the following:

* [PlatformIO](http://platformio.org) (all needed libraries and settings are pre-configured in [platformio.ini](https://github.com/arendst/Sonoff-Tasmota/blob/master/platformio.ini))

* [Arduino IDE](https://www.arduino.cc/en/Main/Software) (needed preparation below)

## Other Requirements

* The willingness to solder and tinker
* The firmware source code found here
* An MQTT broker
* An MQTT client to interact with the Sonoff module (Desktop Client, Android App, Home Automation Software, ...)

----

### Arduino IDE Preparation

Install the ESP8266 Arduino development environment from [esp8266 Arduino](https://github.com/esp8266/Arduino). The software is supported with Arduino IDE starting with version 1.6.10 and esp8266 Arduino stable version 2.3.0.

- I prefer a **dedicated standalone version** of the IDE allowing easy ESP8266 file manipulation and library management. This can be achieved by downloading the Arduino IDE ZIP file for non admin install. After unzipping and before executing ``arduino.exe`` add an empty directory called ``portable``
- Follow the procedure from the ESP8266 Arduino README.md to install the development environment using the Arduino IDE Board Manager
- Copy the ``sonoff`` directory to your sketchfolder
- Download and unzip the [pubsubclient](https://github.com/knolleary/pubsubclient/releases/tag/v2.6) MQTT library **version 2.6** into directory ``portable\sketchbook\libraries`` and rename to ``pubsubclient``. Update default value in file ``pubsubclient\src\PubSubClient.h``  
  - Change ``MQTT_MAX_PACKET_SIZE`` from 128 to at least 512  
- Install the ArduinoJson library **version 5.8.3** via the library manager (Arduino IDE > Sketch > include Library > Manage Libraries) or download and unzip the [ArduinoJson](https://github.com/bblanchon/ArduinoJson/releases/tag/v5.8.3
) library into directory ``portable\sketchbook\libraries`` and rename to ``ArduinoJson``
- If option ``USE_DS18x20`` is enabled in ``user_config.h`` install the OneWire library **version 2.3.3** via the library manager (Arduino IDE > Sketch > include Library > Manage Libraries) or download and unzip the [OneWire](https://github.com/PaulStoffregen/OneWire/releases/tag/v2.3.3) library into directory ``portable\sketchbook\libraries`` and rename to ``OneWire``
- If option ``USE_IR_REMOTE`` is enabled in ``user_config.h`` install the IRremoteESP8266 library **version 1.0.2** via the library manager (Arduino IDE > Sketch > include Library > Manage Libraries) or download and unzip the [IRremoteESP8266](https://github.com/markszabo/IRremoteESP8266/releases/tag/v1.0.2) library into directory ``portable\sketchbook\libraries`` and rename to ``IRremoteESP8266``
- If option ``USE_WS2812`` is enabled in ``user_config.h`` install the NeoPixelBus library **version 2.2.6** via the library manager (Arduino IDE > Sketch > include Library > Manage Libraries) or download and unzip the [NeoPixelBus](https://github.com/Makuna/NeoPixelBus/releases/tag/2.2.6) library  into directory ``portable\sketchbook\libraries`` and rename to ``NeoPixelBus``

Optionally install php and a local web server (ie apache) for OTA and copy directory ``api`` in webroot.

#### "Over The Air" updates
If you want to be able to upload the OTA file from the IDE to your web server perform the following changes to the Arduino IDE environment:

- Copy file ``espupload.py`` to directory ``portable\packages\esp8266\hardware\esp8266\2.3.0\tools`` and change HOST_ADDR to refer to your web server
- Replace files ``boards.txt`` and ``platform.txt`` in directory ``portable\packages\esp8266\hardware\esp8266\2.3.0``

This will provide an additional option ``Tools - Upload Using: OTA_upload``.
