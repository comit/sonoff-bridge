
* Itead Product Page: http://sonoff.itead.cc/en/products/sonoff/sonoff-basic
* Itead Shop: https://www.itead.cc/sonoff-wifi-wireless-switch.html
* Itead Wiki: https://www.itead.cc/wiki/Sonoff

## Serial Connection

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

<img alt="Sonoff Basic connection diagram" src="https://user-images.githubusercontent.com/2870104/30516551-ed12d69e-9b42-11e7-8373-1bfbbf346839.png" width="50%" align="right" />

You need to access the serial interface. The **four serial pins** (3V3, Rx, Tx, GND) are available in the middle of the PCB, right next to the on-board button. Newer version of the Sonoff module provide five pins below the button, ignore the pin furthest away from the Button (GPIO14) if available. The square pin right next to the button is the 3.3V line.

more Info about the GIOS Pins here:
[https://github.com/arendst/Sonoff-Tasmota/wiki/GPIO-Locations](https://github.com/arendst/Sonoff-Tasmota/wiki/GPIO-Locations)

For flashing the sonoff basic V1.1, please hold the button while connecting the Plus Pole. The LED remains off until the flashing process is done and the board is rebooted.

## GPIO Locations

<img alt="GPIO 01,03 and 14" src="https://bytebucket.org/xoseperez/espurna/wiki/images/flashing/sonoff-flash.jpg"/><br/>
<img alt="GPIO 04" src="http://evertdekker.com/wp/wp-content/gallery/sonoff/p1010285.jpg"/><br/>

* GPIO 03 - RX PIN
* GPIO 01 - TX PIN
* GPIO 04 - Second image (must solder wire to pin on ESP chip)
* GPIO 14 - Below GND PIN~