
* Itead Product Page: http://sonoff.itead.cc/en/products/residential/sonoff-b1
* Itead Shop: https://www.itead.cc/sonoff-b1.html
* Itead Wiki: (not available)

## Serial Connection

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. First pop up the top part of the bulb with controlled force. The PCB as shown in the image will become visible.

<img alt="Sonoff B1 PCB" src="https://user-images.githubusercontent.com/2870104/30506986-b5be2882-9a80-11e7-849a-ccd65dd8a1a6.png" width="60%" align="right" />

The **four serial pins** (3V3, Rx, Tx, GND) as well as the GPIO0 signal line are available as test points and clearly marked. Solder wires to those or use pogo pins as you prefer.

As with all modules pulling GPIO0 to GND is needed to put the chip in programming mode. You need to **connect GPIO0 and GND** during power up. An additional GND pad is available in the middle of the PCB.
