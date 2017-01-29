In the Arduino IDE for sonoff select from ```Tools Board Generic ESP8266 Module``` and set the following options:

- Flash Mode: DIO
- Flash Frequency: 40MHz
- Upload Using: Serial
- CPU Frequency: 80MHz
- Flash Size: 1M (64K SPIFFS)
- Debug Port: Disabled
- Debug Level: None
- Reset Method: ck
- Upload Speed: 115200
- Port: Your COM port connected to sonoff

Load the file ```sonoff.ino``` into the IDE.

Verify and/or compile the project and upload once to your sonoff using the serial connection as shown in [Peter Scargill's blog](http://tech.scargill.net/itead-slampher-and-sonoff). See below for specific Sonoff Module information.

### Sonoff Basic

<img alt="Connection diagram" src="https://github.com/arendst/arendst.github.io/blob/master/media/ProgramESP8266.jpg" height="260" /><br/>

*Note:* newer version of the Sonoff module consist of five pins below the button. Follow the image above and ignore the pin furthest to the Button (pin 5) if available.

| Sonoff (starting at button) | Programmer |
|-----------------------------|------------|
|                     1 (VCC) |        3V3 |
|                      2 (RX) |         TX |
|                      3 (TX) |         RX |
|                     4 (GND) |        GND |
|          (5 - if available) |            |

**Do not connect AC power during the flash cable connection**. 

### Sonoff RF
<img alt="S20 Smart Socket" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffrffix.jpg" width="230" align="right" /> 
The Sonoff RF needs some tweeking as the connection needed during programming between the button and GPIO0 might not be present.

[Phalox](http://phalox.be/wp/electronics/itead-sonoff-slampher-custom-firmware-fix/) explains the case with a picture where you have to install a jumper wire for R21. I received the same result using a small screwdriver and shorting both solder pads of R21 while holding down the button during programming.

Pairing a RF remote is the same as with iTead software:
- two short button presses will blink a red led shortly and start RF signal recognition. Three longer blinks signal RF reception.
- three short button presses will keep the red led on for some seconds and erase the known RF code.

I was unable to pair the Sonoff RF 434MHz receiver with any of my KaKu switches so to me the RF functionality is a bit disappointing.

### Sonoff Dual
<img alt="Dual GPIO0 grounded" src="https://github.com/arendst/arendst.github.io/blob/master/media/dual2a.jpg" width="230" align="right" /> 
Programming the Sonoff Dual is also more difficult as the button is not connected to GPIO0 which is needed to put the ESP8266 in programming mode during power up.

I suggest to solder a 4 pin header for the serial interface as shown in the image on the right (the vertical connector) and use the small inter layer VIA to ground GPIO0 using the GND pin from the button 0 and button 1 header.

The 4 pin header in the middle, which is normally not present, is not needed but might be used in programming the ESP8266 as there must be a better way for itead to get the initial code loaded ...

### S20 Smart Socket
<img alt="S20 Smart Socket" src="https://github.com/arendst/arendst.github.io/blob/master/media/s20b.jpg" width="230" align="right" /> 
The picture on the right shows how to program the S20 Smart Socket powered by the FTDI USB converter.

Remember that during programming the Smart Socket is **NOT** connected to mains.

### Sonoff Pow - WARNING
Trying to program the Sonoff Pow [comrade MySKU](http://mysku.ru/blog/china-stores/45762.html) learned the hard way how to brick it.

<img alt="Sonoff Pow Bricked" src="https://github.com/arendst/arendst.github.io/blob/master/media/pow1.jpg" width="230" align="right" /> 
As the Sonoff Pow power monitoring hardware connects AC power to the logic ground of the ESP8266 it is utterly mandatory to **NOT CONNECT AC POWER WHILE SERIAL CONNECTION IS BEING USED**.

During both AC connection and Serial connection you may connect the life AC wire with your PC's DC ground leading to a power short, broken Sonoff Pow and laptop Power Supply as MySKU has experienced.

### Sonoff Touch
<img alt="Sonoff Touch EU" src="https://github.com/arendst/arendst.github.io/blob/master/media/toucheu.jpg" width="230" align="right" /> 
As the Sonoff Touch is based on the ESP8285 using Flash Mode DOUT you will have to make some changes to the proposed Arduino IDE settings as follows:

- Tools Board Generic ESP8285 Module
- Flash Size: 1M (64K SPIFFS)

Programming the Sonoff touch is as easy as the Sonoff Basic.

<img alt="Sonoff Touch US" src="https://github.com/arendst/arendst.github.io/blob/master/media/touchus.jpg" width="230" align="right" /> 
Remove the top PCA containing the ESP8285 from the assembly as shown in the pictures on the right.

The pictures show for both the EU version (top) and the US version (bottom) where to connect your FTDI cable (Gnd, TxD, RxD and 3.3V). The GPIO0 pin needs to be connected to Ground to put the Sonoff Touch in programming mode.

Remember that during programming the Sonoff Touch is **NOT** connected to mains.

### iTead Motor Clockwise/Anticlockwise
<img alt="MotorCAC" src="https://github.com/arendst/arendst.github.io/blob/master/media/motorcac1.jpg" width="230" align="right" /> 
This USB powered or external powered board provides one GPIO controlling two alternating relays with Normally Open (NO) and Normally Closed (NC) contacts. It can be used for changing directions of a connected motor.

Programming the onboard 3.3V [PSA-B](https://www.itead.cc/psa-01.html) is possible when Rx (pin7), Tx (pin8) and Gnd (pin9) are connected to the FTDI interface, the button is pressed and (USB) power is provided.

### Upload image
Verify and upload an OTA image to your web server with option ```Upload Using: OTA_upload```.

Since version 1.0.26 you may also use sonoffs web server and upload the file directly.

Enable debug messages with command ```seriallog 3```.