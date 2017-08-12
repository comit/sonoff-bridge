
* Itead Product Page: http://sonoff.itead.cc/en/products/residential/sonoff-touch
* Itead Shop: https://www.itead.cc/sonoff-touch.html
* Itead Wiki: (na)

Other than most Sonoff modules (ESP8266) the Sonoff Touch is based on the ESP8285. Though the actual chip inside may be a [PSF-A85](https://www.itead.cc/wiki/PSF-A85).

## Serial Connection

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. Carefully remove the top PCA from the assembly. The hidden underside of the PCB contains the ESP8285 as shown in the pictures. The **four serial pins** (3V3, Rx, Tx, GND) can be seen in the pictures for the US version (left) and the EU version (right) of the module PCB.

<img title="Sonoff Touch US version" src="https://github.com/arendst/arendst.github.io/blob/master/media/touchus.jpg" width="44%" /> 
<img title="Sonoff Touch EU version" src="https://github.com/arendst/arendst.github.io/blob/master/media/toucheu.jpg" width="48%" align="right" />

Be careful while removing and reassembling the top PCB. The touch sensor should be back in its intended place, be sure to not touch it directly during the modifications.

The Sonoff Touch button is not connected to **GPIO0** and can hence not be used to bring the module into [Programming Mode](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation#bringing-the-module-in-flash-mode). A connection between GPIO0 and GND needs to be made manually. GPIO0 can be found on the right side of the ESP8285 and is the second pin from the bottom, as can be seen on the pictures.

**Note:** Even if you have the PSF-A85 chip inside instead of a default ESP-8285, the GPIO0 pin is in the same location. Pay attention to the corner of the chip with three unused solder contacts. That is where the external antenna connector is located in the images above. The PSF-A85 in the Sonoff Touch does not have the external antenna connector soldered on.

### Arduino IDE Modifications

As the Sonoff Touch is based on the ESP8285 using Flash Mode DOUT you will have to make some changes to the [proposed](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisite) Arduino IDE settings as follows:

- Tools → Board → Generic ESP8285 Module
- Flash Size: 1M (64K SPIFFS) - version 4.x.x and before 
- Flash Size: 1M (no SPIFFS) - version 5.x.x and following

### PlatformIO

Appropriate settings are available in the PlatformIO configuration file provided with this firmware.
Even though the settings _seem_ to refer to the wrong chip, the upload script will detect the chip type and flash it in an appropriate manner, see [below](#Flashing_the_ESP-8285)

### Flashing the ESP-8285
Where most Sonoffs are ESP8266 based the Sonoff Touch and Sonoff 4CH are based on the ESP8285.

Starting with version 3.9.12 the OTA or Upload code will now check if one of the above modules is selected and patch the uploaded default ESP8266 code with FlashChipMode DIO to the ESP8285 FlashChipMode DOUT before it is copied to it's final destination.

After first restart make sure to select either module Sonoff Touch or Sonoff 4CH **before any OTA or Upload** action.