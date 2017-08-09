* Itead Product Page: http://sonoff.itead.cc/en/products/appliances/sonoff-rf-bridge-433
* Itead Shop: https://www.itead.cc/sonoff-rf-bridge-433.html
* Itead Wiki: https://www.itead.cc/wiki/Sonoff_RF_Bridge_433

## Serial Connection

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff_bridge_2.jpg" width="250" align="right" />

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. The **four serial pins** (3V3, Rx, Tx, GND) connected to the esp8285 are available on the 5-pin header just next to the switch as can be seen in the image to the right.

Move the switch towards the 5-pin header, keep the button pressed and connect the serial programmer.

After programming make sure to move the switch away from the 5-pin header to restore connection to the RF microcontroller.

## Operation

During normal operation the serial interface is used at 19200 baud to communicate with the RF microcontroller. It is therefore wise to disable serial logging (``seriallog 0``).

The bridge is able to learn up to 16 different remote control commands of fixed code 433 MHz frequency as provided by PT2260, PT2262, PT2264 and EV1527 Transmitters. It does not recognize Klik Aan Klik Uit (KaKu) remote control signals.

Tasmota provides default remote control commands to all 16 keys so you can start using the bridge with a Sonoff 4CH Pro or Sonoff RF device without having it to learn remote control commands.

See [Supported Commands](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#sonoff-rf-bridge-433) for available specific Sonoff RF Bridge 433 commands.