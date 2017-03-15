## Hardware Preparation

You need to make the serial programming interface of the Sonoff module / the ESP8266 microchip available. Examples are shown in in [Peter Scargill's blog](http://tech.scargill.net/itead-slampher-and-sonoff) or by [captain-slow.dk](http://captain-slow.dk/2016/05/22/replacing-the-itead-sonoff-firmware/). In most cases the pins are available on the PCB but connectors need to be soldered to allow interfacing. You'll furthermore need a **3.3V FTDI USB-to-Serial Converter/Programmer**.

The following table shows the connection for most Sonoff modules:

| Sonoff             | Programmer |
|--------------------|------------|
| 1 (VCC)            |        3V3 |
| 2 (RX)             |         TX |
| 3 (TX)             |         RX |
| 4 (GND)            |        GND |
| (5 - if available) |            |

### Attention

**⚠️️⚠️️⚠️️ Do not connect AC power and the serial connection at the same time ⚠️️⚠️️⚠️️** 

Shorting your serial interface with AC will fry your module, programmer and even your PC.
If you are not careful, your own health might be in danger.

### Bringing the Module in Flash Mode

The "brain" of the Sonoff Module (normally the ESP8266) needs to be put into Flash Mode. This is done, by pulling the GPIO0 pin to GND while the chip is booting. On most modules the installed control button is connected to GPIO0 and GND, making entering Flash Mode very easy. On other modules you will need to connect pins on the PCB.

To bring a Sonoff module into Flash Mode:

1. Disconnect serial programmer and power)
2. Connect GPIO0 and GND (e.g. Press the button)
3. Connect serial programmer
4. Release GPIO0 (after one-two seconds)

## Sonoff Module Specifics

See below for specific Sonoff Module information and examples.

### Sonoff Basic

<img alt="Connection diagram" src="https://github.com/arendst/arendst.github.io/blob/master/media/ProgramESP8266.jpg" height="260" /><br/>

*Note:* newer version of the Sonoff module hold five pins below the button. Follow the image above and ignore the pin furthest to the Button (pin 5) if available.

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

Pairing a RF remote is the same as with iTead software:
- two short button presses will blink a red led shortly and start RF signal recognition. Three longer blinks signal RF reception.
- three short button presses will keep the red led on for some seconds and erase the known RF code. 

I was unable to pair the Sonoff RF 434MHz receiver with any of my KaKu switches so to me the RF functionality is a bit disappointing.

### Sonoff Dual

<img alt="Dual GPIO0 grounded" src="https://github.com/arendst/arendst.github.io/blob/master/media/dual2a.jpg" width="230" align="right" /> 
Programming the Sonoff Dual is more difficult as the on-board-button is not connected to GPIO0. Pulling GPIO0 to GND is needed to put the ESP8266 in programming mode during power up.

As always, you need to solder a 4 pin header for the serial interface (connector on the short end of the PCB). GPIO0 can be found in the small inter layer [via](https://en.wikipedia.org/wiki/Via_(electronics)) shown in the image. Using the GND pin from the button 0 / button 1 header connect the via during power up. Attention: If the via is covered by silk screen (green) you need to expose the underlying conductive (copper) by careful scratching.

The 4 pin header in the middle, which is normally not present, is not needed but might be used in programming the ESP8266 as there must be a better way for itead to get the initial code loaded ...

### Sonoff Pow

<!-- this can be deleted. General warning now given above -->

Trying to program the Sonoff Pow [comrade MySKU](http://mysku.ru/blog/china-stores/45762.html) learned the hard way how to brick it.

<img alt="Sonoff Pow Bricked" src="https://github.com/arendst/arendst.github.io/blob/master/media/pow1.jpg" width="230" align="right" /> 
As the Sonoff Pow power monitoring hardware connects AC power to the logic ground of the ESP8266 it is utterly mandatory to **NOT CONNECT AC POWER WHILE SERIAL CONNECTION IS BEING USED**.

During both AC connection and Serial connection you may connect the life AC wire with your PC's DC ground leading to a power short, broken Sonoff Pow and laptop Power Supply as MySKU has experienced.

A solution that works for me is shown in the schematic below using two opto couplers seperating the AC connection on the **left** from the low voltage connection on the **right**. This allows for serial control at 115200 baud and uploading of firmware up to 57600 baud while AC is connected.
<img alt="OptoSerial" src="https://github.com/arendst/arendst.github.io/blob/master/media/OptoSerial.jpg" /> 

### Sonoff Touch

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/toucheu.jpg" width="230" align="right" /> 
As the Sonoff Touch is based on the ESP8285 using Flash Mode DOUT you will have to make some changes to the proposed Arduino IDE settings as follows:

- Tools Board Generic ESP8285 Module
- Flash Size: 1M (64K SPIFFS)



Programming the Sonoff touch is as easy as the Sonoff Basic.

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/touchus.jpg" width="230" align="right" /> 

Remove the top PCA containing the ESP8285 from the assembly as shown in the pictures on the right.

The pictures show for both the EU version (top) and the US version (bottom) where to connect your FTDI cable (Gnd, TxD, RxD and 3.3V). The GPIO0 pin needs to be connected to Ground to put the Sonoff Touch in programming mode.

### Motor Clockwise/Anticlockwise

<img alt="MotorCAC" src="https://github.com/arendst/arendst.github.io/blob/master/media/motorcac1.jpg" width="230" align="right" /> 
This USB powered or external powered board provides one GPIO controlling two alternating relays with Normally Open (NO) and Normally Closed (NC) contacts. It can be used for changing directions of a connected motor.

Programming the on-board 3.3V [PSA-B](https://www.itead.cc/psa-01.html) is possible when Rx (Pin 7), Tx (Pin 8) and GND (Pin 9) are connected to the FTDI interface, the button is pressed and (USB) power is provided.

### Sonoff 4ch

On the PCB the `TX` and `RX` pins are mislabeled. The `TX` of the programmer needs to connect to the `TX` on the 4ch PCB and the `RX` to the `RX`. Normally this is reversed.

Note to set the board to "Generic ESP8285 Module".