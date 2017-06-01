
* Itead Product Page: http://sonoff.itead.cc/en/products/residential/sonoff-touch
* Itead Shop: https://www.itead.cc/sonoff-touch.html
* Itead Wiki: (na)

## Serial Connection

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. The **four serial pins** (3V3, Rx, Tx, GND) are available at the short end of the PCB and can be seen on the left side of the first image and are labeled in red on the second image.

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/toucheu.jpg" width="230" align="right" />
As the Sonoff Touch is based on the ESP8285 using Flash Mode DOUT you will have to make some changes to the proposed Arduino IDE settings as follows:

- Tools Board Generic ESP8285 Module
- Flash Size: 1M (64K SPIFFS)

⚠️️ As of version 5.x.x Flash Size should be set to -> **"1M (no SPIFFS)"** ⚠️️
Procedure explained in [Prerequisite](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisite) section of wiki

<br />
Programming the Sonoff touch is as easy as the Sonoff Basic.

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/touchus.jpg" width="230" align="right" /> 

Remove the top PCA containing the ESP8285 from the assembly as shown in the pictures on the right.

The pictures show for both the EU version (top) and the US version (bottom) where to connect your FTDI cable (Gnd, TxD, RxD and 3.3V). The GPIO0 pin needs to be connected to Ground to put the Sonoff Touch in programming mode.

----

Where most Sonoff's are ESP8266 based the Sonoff Touch and Sonoff 4CH are based on the ESP8285.

It is important to initially flash these devices with the correct FlashChipMode of DOUT as otherwise future OTA or Upload updates will fail.

In the Arduino IDE select Board Generic ESP8285 Module as this contains the default DOUT option. You could still use the Generic ESP8266 module as long as you set parameter Flash Mode to DOUT.

After first restart make sure to select either module Sonoff Touch or Sonoff 4CH at least before any OTA or Upload action.

Starting with version 3.9.12 the OTA or Upload code will now check if one of the above modules is selected and patch the uploaded default ESP8266 code with FlashChipMode DIO to the ESP8285 FlashChipMode DOUT before it is copied to it's final destination.