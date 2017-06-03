
* Itead Product Page: http://sonoff.itead.cc/en/products/residential/sonoff-touch
* Itead Shop: https://www.itead.cc/sonoff-touch.html
* Itead Wiki: (na)

Other than most Sonoff modules (ESP8266) the Sonoff Touch is based on the ESP8285.

## Serial Connection

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. Carefully remove the top PCA from the assembly. The hidden underside of the PCB contains the ESP8285 as shown in the pictures. The **four serial pins** (3V3, Rx, Tx, GND) can be seen in the pictures for the US version (left) and the EU version (right) of the module PCB.

<img title="Sonoff Touch US version" src="https://github.com/arendst/arendst.github.io/blob/master/media/touchus.jpg" width="44%" /> 
<img title="Sonoff Touch EU version" src="https://github.com/arendst/arendst.github.io/blob/master/media/toucheu.jpg" width="48%" align="right" />

Be careful while removing and reassembling the top PCB. The touch sensor should be back in its intended place, be sure to not touch it directly during the modifications.

The Sonoff-Touch button is not connected to **GPIO0** and can hence not be used to bring the module into [Programming Mode](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation#bringing-the-module-in-flash-mode). A connection between GPIO0 and GND needs to be made manually. GPIO0 can be found on the right side of the ESP8285 and is the second pin from the bottom, as can be seen on the pictures.

### Arduino IDE Modifications

As the Sonoff Touch is based on the ESP8285 using Flash Mode DOUT you will have to make some changes to the [proposed](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisite) Arduino IDE settings as follows:

- Tools → Board → Generic ESP8285 Module
- Flash Size: 1M (64K SPIFFS) - version 4.x.x and before 
- Flash Size: 1M (no SPIFFS) - version 5.x.x and following

Appropriate settings are available in the PlatformIO configuration file provided with this firmware.

----
*The following can probably be deleted:*

Where most Sonoff's are ESP8266 based the Sonoff Touch and Sonoff 4CH are based on the ESP8285.

It is important to initially flash these devices with the correct FlashChipMode of DOUT as otherwise future OTA or Upload updates will fail.

In the Arduino IDE select Board Generic ESP8285 Module as this contains the default DOUT option. You could still use the Generic ESP8266 module as long as you set parameter Flash Mode to DOUT.

After first restart make sure to select either module Sonoff Touch or Sonoff 4CH at least before any OTA or Upload action.

Starting with version 3.9.12 the OTA or Upload code will now check if one of the above modules is selected and patch the uploaded default ESP8266 code with FlashChipMode DIO to the ESP8285 FlashChipMode DOUT before it is copied to it's final destination.