**Sonoff 4CH**

* Itead Product Page: http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch
* Itead Shop: https://www.itead.cc/sonoff-4ch.html
* Itead Wiki: https://www.itead.cc/wiki/Sonoff_4CH

Other than most Sonoff modules (ESP8266) the Sonoff 4CH is based on the ESP8285.

**Sonoff 4CH Pro**

* Itead Product Page: http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch-pro
* Itead Shop: https://www.itead.cc/sonoff-4ch-pro.html
* Itead Wiki: https://www.itead.cc/wiki/Sonoff_4CH_Pro

Compared to the 4CH the main differences/improvements of the 4CH Pro are:

  - Relays are isolated from mains and can each switch their own circuit (mains or low voltage).
  - With stock firmware special modes are supported (stand-alone schedules, inching, interlocking).
  - RF receiver (optional key fob required).
  - Dual microcontroller, both a ESP8285 and a STM32.

## Serial Connection

### Sonoff 4CH

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. The **four serial pins** (3V3, Rx, Tx, GND) can be seen in the first picture.
*Attention:* The printed labels on the PCB for Rx and Tx are incorrectly swapped as can be seen on the image.

<img title="Sonoff 4CH serial connection pins" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff4ch_pins.jpg" width="48%" /> 
<img title="Sonoff 4CH GPIO0" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff4ch_gpio0.jpg" width="42%" align="right" />

The Sonoff 4CH features four hardware buttons of which one is already connected to GPIO0, which hence can be used to bring the module into [Programming Mode](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation#bringing-the-module-in-flash-mode). The button is shown on the second picture.

### Sonoff 4CH Pro 

The "FW/IO0" button (Switch 1) is not directly connected to GPIO0 of the ESP module. A different method has to be used to program this board.

To program the ESP chip disconnect power from the board, connect a cable from any Ground (GND) pin to the GPIO0 pin on the ESP (be careful not to touch any of the other pins). This is the second pin to the right on the top row of pins (see picture). While holding the pin connected power on the board. The board does not respond to any button pressed when in programming mode and LED 1, 2 and 3 are on (might differ per board). 

Use the ESP programming header as described in the picture to upload the firmware and follow regular programming procedure.

<img title="Sonoff 4CH Pro programming" src="https://github.com/arendst/arendst.github.io/blob/master/media/4chpro_gpio0.JPG" width="100%" align="middle" />


## Solving OTA and Upload Problems

Where most Sonoff's are ESP8266 based the Sonoff Touch and Sonoff 4CH are based on the ESP8285.

It is important to initially flash these devices with the correct FlashChipMode of DOUT as otherwise future OTA or Upload updates will fail.

In the Arduino IDE select **Board** Generic ESP8285 Module as this contains the default DOUT option. You could still use the Generic ESP8266 module as long as you set parameter ``Flash Mode`` to **DOUT**.

Starting with version 3.9.12 after first restart make sure to select either module Sonoff Touch or Sonoff 4CH at least before any OTA or Upload action.

The OTA or Upload code will now check if one of the above modules is selected and patch the uploaded default ESP8266 code with FlashChipMode DIO to the ESP8285 FlashChipMode DOUT before it is copied to it's final destination.

Starting with version 5.3.0 all OTA or Upload code will be patched for FlashChipMode DOUT whatever Module is selected.

## 4CH Pro DIPSwitch Configuration

Most special modes of the 4CH Pro are controlled by DIP switch panels on the board.
Please refer to the back of the board or the Sonoff documentation for more details.
For normal operation with Tasmota the following settings are recommended:

- S6: 1
- K5: all 1
- K6: all 0

(0 and 1 are printed onto the board next to the switch names)

Changing these switches for operations like inching and interlocking are also supported with Tasmota.