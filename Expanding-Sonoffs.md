One capability of Tasmota is that you can connect additional things to available pins on the ESP8266 that controls these devices

If a pin is defined as GPIO_USER in the module template, you can assign it one of the following functions:
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
* GPIO_SENSOR_END


# Examples

1.    If you take a Sonoff Basic and connect a switch between pin4 (ground) and pin5 (GPIO4) of the 5 pin programming header you now have a second switch connected to the device. You can set this through the module config page or from the command line with
    `gpio 4 switch1`

    now when you hit the built-in switch you should get power1 instead of just power, and if you hit the second switch you will get power2
    you can set the mode of each switch individually with switchmode1 or switchmode2

2.    Instead of connecting a switch, you could connect a 4-pin 2.5mm jack, with the pins wired:
    * tip pin5 (GPIO4)
    * r1 no connection
    * r2 pin1 (3.3v)
    * r3 pin4 (ground)

    You can then plug a sensor into the jack like you would to a Sonoff TH10/TH16 and define what sensor you have connected to GPIO4