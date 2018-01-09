## Hardware Preparation

You need to make the serial programming interface of the Sonoff module / the ESP8266 microchip available. In most cases the pins are available on the PCB but connectors need to be soldered or otherwise applied to allow interfacing. You'll furthermore need a **USB UART TTL 3.3V Converter/Programmer** (e.g. CP2102, CH340G, FT232, PL2303).

Be careful - some so called 3V3 FTDI cables actually supply 5v on the VCC line.  I suggest you measure before connecting to your Sonoff.

<img alt="Sonoff Pow Bricked" src="https://github.com/arendst/arendst.github.io/blob/master/media/pow1.jpg" width="40%" align="right" />

### Attention ⚠️️⚠️️⚠️️

**Do not connect AC power and the serial connection at the same time** 

Shorting your serial interface with AC will fry your module or programmer and may also harm or destroy your PC.
If you are not careful, your own health might be in danger. Always make sure to have all high power cables disconnected from the Sonoff module while being connected via serial/USB or even while the case of the module is opened.

## Serial Connection

The following table shows the connection between the connectors of the programmer and the modules. Pay attention to crossed RX and TX lines.

|Programmer  | Sonoff Module      |
|------------|--------------------|
|        3V3 | 3V3 / VCC          |
|         TX | RX                 |
|         RX | TX                 |
|        GND | GND                |

Some Sonoff modules expose a five pin header. The fifth pin is irrelevant for the serial connection. Please check the specific module pages to find out more.

### Bringing the Module in Flash Mode

The "brain" of the Sonoff module (normally the ESP8266) needs to be put into Flash Mode before the Sonoff-Tasmota firmware can be transferred. This is done by pulling the GPIO0 pin to GND while the chip is booting. On most modules the installed control button is connected to GPIO0 and GND, making entering Flash Mode very easy. On other modules you will need to connect pins on the PCB. See device specific articles for details.

To bring a Sonoff module into Flash Mode:

1. Disconnect serial programmer and power
2. Connect GPIO0 and GND (e.g., by pressing the on-board button or connection via cable)
3. Connect the serial programmer (VCC, RX, TX, GND), e.g. by connecting the belonging USB cable
4. Disconnect GPIO0 from GND (after one-two seconds)

If everything went well, you are now in Flash Mode and ready to continue with the Sonoff-Tasmota firmware [Upload](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload). If the upload is not able to start, disconnect the module and start the hardware preparations from the beginning.

### Module Specific Articles

Check the module specific instructions and restrictions documented in the **device specific articles linked in the menu on the right** 🡺

* [Sonoff Basic](Sonoff-Basic#serial-connection)
* [Sonoff Dual](Sonoff-Dual)
* [Sonoff RF](Sonoff-RF#serial-connection)
* [Sonoff Pow](Sonoff-Pow)
* [Sonoff Touch](Sonoff-Touch)
* [Sonoff 4CH / 4CH Pro](Sonoff-4CH-and-4CH-Pro)
* ... (see menu on the right)

