
* Itead Product Page: http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch
* Itead Shop: https://www.itead.cc/sonoff-4ch.html
* Itead Wiki: https://www.itead.cc/wiki/Sonoff_4CH

Other than most Sonoff modules (ESP8266) the Sonoff 4CH is based on the ESP8285.

## Serial Connection

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. The **four serial pins** (3V3, Rx, Tx, GND) can be seen in the first picture.
*Attention:* The printed labels on the PCB for Rx and Tx are incorrectly swapped as can be seen on the image.

<img title="Sonoff 4CH serial connection pins" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff4ch_pins.jpg" width="48%" /> 
<img title="Sonoff 4CH GPIO0" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff4ch_gpio0.jpg" width="40%" align="right" />

The Sonoff 4CH features four hardware buttons of which one is already connected to GPIO0, which hence can be used to bring the module into [Programming Mode](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation#bringing-the-module-in-flash-mode). The button is shown on the second picture.


## Solving OTA and Upload Problems
Where most Sonoff's are ESP8266 based the Sonoff Touch and Sonoff 4CH are based on the ESP8285.

It is important to initially flash these devices with the correct FlashChipMode of DOUT as otherwise future OTA or Upload updates will fail.

In the Arduino IDE select **Board** Generic ESP8285 Module as this contains the default DOUT option. You could still use the Generic ESP8266 module as long as you set parameter ``Flash Mode`` to **DOUT**.

After first restart make sure to select either module Sonoff Touch or Sonoff 4CH at least before any OTA or Upload action.

Starting with version 3.9.12 the OTA or Upload code will now check if one of the above modules is selected and patch the uploaded default ESP8266 code with FlashChipMode DIO to the ESP8285 FlashChipMode DOUT before it is copied to it's final destination.
