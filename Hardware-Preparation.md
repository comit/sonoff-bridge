## Hardware Preparation

You need to make the serial programming interface of the Sonoff module / the ESP8266 microchip available. In most cases the pins are available on the PCB but connectors need to be soldered to allow interfacing. You'll furthermore need a **3.3V FTDI USB-to-Serial Converter/Programmer**.

<img alt="Sonoff Pow Bricked" src="https://github.com/arendst/arendst.github.io/blob/master/media/pow1.jpg" width="40%" align="right" />

### Attention ⚠️️⚠️️⚠️️

**Do not connect AC power and the serial connection at the same time** 

Shorting your serial interface with AC will fry your module or programmer and may also harm or destroy your PC.
If you are not careful, your own health might be in danger. Always make sure to have all high power cables disconnected from the Sonoff module while being connected via serial/USB or even while the case of the module is opened.

## Serial Connection

The following table shows the connection for most Sonoff modules:

|Programmer  | Sonoff Module      |
|------------|--------------------|
|        3V3 | 3V3 / VCC          |
|         TX | RX                 |
|         RX | TX                 |
|        GND | GND                |

Pay attention to the fact, that RX and TX are crossed.

Some Sonoff modules expose a five pin header. The fifth pin is irrelevant for the serial connection. Please check the specific module pages to find out more.

Examples of the above preparations are shown in [Peter Scargill's blog](http://tech.scargill.net/itead-slampher-and-sonoff) or by [captain-slow.dk](http://captain-slow.dk/2016/05/22/replacing-the-itead-sonoff-firmware/).

### Sonoff Module Specifics

For module specific instructions and restrictions of the individual devices, please see the respective articles:

* [Sonoff Dual](https://github.com/arendst/Sonoff-Tasmota/wiki/Sonoff-Dual)
* [Sonoff Pow](https://github.com/arendst/Sonoff-Tasmota/wiki/Sonoff-Pow)
* [Sonoff Touch](https://github.com/arendst/Sonoff-Tasmota/wiki/Sonoff-Touch)
* [Sonoff 4CH / 4CH Pro](https://github.com/arendst/Sonoff-Tasmota/wiki/Sonoff-4CH-and-4CH-Pro)

### Bringing the Module in Flash Mode

The "brain" of the Sonoff module (normally the ESP8266) needs to be put into Flash Mode. This is done, by pulling the GPIO0 pin to GND while the chip is booting. On most modules the installed control button is connected to GPIO0 and GND, making entering Flash Mode very easy. On other modules you will need to connect pins on the PCB. See device specific articles for details.

To bring a Sonoff module into Flash Mode:

1. Disconnect serial programmer and power
2. Connect GPIO0 and GND (e.g., by pressing the on-board button or connection via cable)
3. Connect the serial programmer (VCC, RX, TX, GND)
4. Disconnect GPIO0 from GND (after one-two seconds)

If everything went well, you are now in Flash Mode and ready to continue with the Sonoff-Tasmota firmware [Upload](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload). If the upload is not able to start, disconnect the module and start the hardware preparations from the beginning.

----

### Sonoff Basic

<img alt="Connection diagram" src="https://github.com/arendst/arendst.github.io/blob/master/media/ProgramESP8266.jpg" height="260" /><br/>

*Note:* newer version of the Sonoff module hold five pins below the button. Follow the image above and ignore the pin furthest away from the Button (pin 5) if available. The square pin is 3.3V.

### S20 Smart Socket

<img alt="S20 Smart Socket" src="https://github.com/arendst/arendst.github.io/blob/master/media/s20b.jpg" width="230" align="right" /> 
The picture shows how to program the S20 Smart Socket powered by the FTDI USB converter.
<br />
<br />
<br />
<br />

### Sonoff RF

The Sonoff RF needs some tweaking as the connection needed during programming between the button and GPIO0 might not be present.

[Phalox](http://phalox.be/wp/electronics/itead-sonoff-slampher-custom-firmware-fix/) explains the case with a picture where you have to install a jumper wire for R21. I received the same result using a small screwdriver and shorting both solder pads of R21 while holding down the button during programming.

<img alt="S20 Smart Socket" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffrffix.jpg" width="230" align="right" />

Pairing the iTead RF remote controller is the same as with the original iTead software:
- two short button presses will blink a red led shortly and start RF signal recognition. Three longer blinks signal RF reception.
- three short button presses will keep the red led on for some seconds and erase the known RF code. 

I was unable to pair the Sonoff RF 434MHz receiver with my KaKu switches but the iTead provided remote control works just fine.

### Motor Clockwise/Anticlockwise

<img alt="MotorCAC" src="https://github.com/arendst/arendst.github.io/blob/master/media/motorcac1.jpg" width="230" align="right" /> 
This USB powered or external powered board provides one GPIO controlling two alternating relays with Normally Open (NO) and Normally Closed (NC) contacts. It can be used for changing directions of a connected motor.

Programming the on-board 3.3V [PSA-B](https://www.itead.cc/psa-01.html) is possible when Rx (Pin 7), Tx (Pin 8) and GND (Pin 9) are connected to the FTDI interface, the button is pressed and (USB) power is provided.

### Sonoff SC

Flashing the ESP8266

Remove the 4 screws on the bottom.
The button is connected to GPIO0.

You will have to remove the TX jumper in the board to avoid the ATMega328P to interfere in the upload process.

Press and hold the button while powering the board to set the ESP8266 into flashing mode.
Note! After flashing you need to set the baudrate to 19200.
Don't forget to reconnect the TX jumper after flashing ;)

<img alt="SonoffSC" src="https://puu.sh/vZZRI/ff36ff9244.jpg" width="230" />
<img alt="SonoffScRemoveTX" src="https://puu.sh/vZZSi/43244f3cc1.jpg" width="230" /> 
<img alt="SonoffSCButoom" src="https://puu.sh/vZZSC/aaa140afa3.jpg" width="130" />
