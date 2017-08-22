Using the [Sleep command](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#management) you can instruct Tasmota to sleep for the set milliseconds in its main cycle. While sleeping your device will consume less power.

Setting this around 50 ms reduces power consumption from ~1.1W to ~0.4W on an idling (relay off) sonoff and button presses are still noticed correctly. With this setting you have to concentrate very hard to click the button so fast that it is not recognized by the device.

If your Sonoff needs to do something continuously, this will be affected. E.g. Sonoff LED will flicker and Sonoff POW will not be able to correctly total the energy consumption. Spot readings of power, voltage, etc. will however remain correct.

> From the release notes:
> 
> Expect overall button/key/switch misses and wrong values on Sonoff Pow