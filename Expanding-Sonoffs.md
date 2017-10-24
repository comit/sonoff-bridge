One capability of Tasmota is that you can connect additional things to available pins on the ESP8266 that controls these devices

If a pin is defined as GPIO_USER in the module template, you can assign it one of the following functions (as of 3/9/17 version 4.0.3):
```
* GPIO_NONE,           // Not used
* GPIO_DHT11,          // DHT11
* GPIO_DHT21,          // DHT21, AM2301
* GPIO_DHT22,          // DHT22, AM2302, AM2321
* GPIO_DSB,            // Single wire DS18B20 or DS18S20
* GPIO_I2C_SCL,        // I2C SCL
* GPIO_I2C_SDA,        // I2C SDA
* GPIO_WS2812,         // WS2812 Led string
* GPIO_IRSEND,         // IR remote
* GPIO_SWT1,           // User connected external switches
* GPIO_SWT2,
* GPIO_SWT3,
* GPIO_SWT4,
* GPIO_KEY1,           // Button usually connected to GPIO0
* GPIO_KEY2,
* GPIO_KEY3,
* GPIO_KEY4,
* GPIO_REL1,           // Relays (0 = off, 1 = on)
* GPIO_REL2,
* GPIO_REL3,
* GPIO_REL4,
* GPIO_REL1_INV,       // Relays with inverted signal control (0 = on, 1 = off)
* GPIO_REL2_INV,
* GPIO_REL3_INV,
* GPIO_REL4_INV,
* GPIO_LED1,           // Leds (0 = off, 1 = on)
* GPIO_LED2,
* GPIO_LED3,
* GPIO_LED4,
* GPIO_LED1_INV,       // Leds with inverted signal control (0 = on, 1 = off)
* GPIO_LED2_INV,
* GPIO_LED3_INV,
* GPIO_LED4_INV
```

To make a link between the different naming schemes of pins, connectors and logical functions, the [Pin Definition overview](https://github.com/esp8266/esp8266-wiki/wiki/Pin-definition) in the esp8266 wiki is quite helpful.

# Examples
## Connect switch
If you take a Sonoff Basic and connect a switch between pin4 (ground) and pin5 (GPIO14) of the 5 pin programming header you now have a second switch connected to the device. You can set this through the module config page as option ``09 Switch1`` or from the command line with ``gpio14 9``.

If you have fewer than two relays on the module, the additional switch(es) will not show up any different than the built-in switch and will control the single relay unless you set switchtopic to something other than 0 (either 1 or a custom topic). Once this is done the built-in switch will produce ``stat/<topic>/POWER1`` while the new switch will produce ``cmnd/<switchtopic>/POWER1``

With more relays on the modules, the additional switch will create additional POWER\<n> events without the need to set switchtopic.

You can set the mode of each switch individually with ``switchmode1`` or ``switchmode2``

See [SwitchMode and SwitchTopic](Understanding-SwitchMode-and-SwitchTopic) for more information.

## Connect jack
Instead of connecting a switch, you could connect a 4-pin 2.5mm jack, with the pins wired:
* tip pin5 (GPIO14)
* r1 no connection
* r2 pin1 (3.3v)
* r3 pin4 (ground)

You can then plug a sensor into the jack like you would to a Sonoff TH10/TH16 and define what sensor you have connected to GPIO14

## Electrical considerations

When you switch a GPIO pin to an input and hang a long wire off of it, that wire can pick up stray signals and cause the voltage on the GPIO pin to vary. This can cause the system to think the switch has changed.

To fix this, there are several things you can do.

1. add a pull-up resistor
1. add a bypass capacitor
1. shielding on the wire
1. use twisted pair wiring

A pull-up resistor is a resistor connected between the GPIO pin and 3.3v. The exact value of this is not critical, 4.7k is a common value to use, as is 10k. This ensures that when the switch it open, the GPIO pin will go high.

A bypass capacitor is a small (pF range) capacitor that is connected between the GPIO and ground. This provides a path for any radio signals that are picked up by the wire to go to ground and not confuse the system.

Shielding or using twisted pair wiring are other ways to reduce the effect of radio signals on the system.