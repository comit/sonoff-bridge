The software allows for dynamically configuration of sensors and devices to selected GPIO pins. Depending on the type of (Sonoff) Module certain GPIO pins are easily accessable.

Sensor configuration is possible by either the web pages OR using the commands ``modules``, ``module``, ``gpios`` and ``gpio``.

The software supports:
- [single wire sensors](#single-wire-sensor)
- [dual wire or I2C sensors](#i2c-sensor)
- [single wire devices](#device)

Only one sensor per group is allowed.

## Single wire sensor
The following single wire sensors are supported:
- DHT11 Temperature and Humidity - ``DHT11 (1)`` in group 1
- DHT21 Temperature and Humidity - ``AM2301 (2)`` in group 1
- AM2301 Temperature and Humidity - ``AM2301 (2)`` in group 1
- DHT22 Temperature and Humidity - ``DHT22 (3)`` in group 1
- AM2302 Temperature and Humidity - ``DHT22 (3)`` in group 1
- AM2321 Temperature and Humidity - ``DHT22 (3)`` in group 1
- DS18B20 Temperature - ``DS18x20 (4)`` in group 2<br/>Enable option ``USE_DS18x20`` in ``user_config.h`` for multiple sensors using OneWire library
- DS18S20 Temperature - ``DS18x20 (4)`` in group 2<br/>Enable option ``USE_DS18x20`` in ``user_config.h`` using OneWire library
- External switch - ``Switch (9)``<br/>Use ``SwitchMode`` to tune it's behaviour
- HC-SR501 PIR Motion Detection - ``Switch (9)``<br/>Use ``SwitchMode`` to tune it's behaviour

You can add a sensor using the following (MQTT) commands:

1. ``modules`` - show supported modules
2. ``module 4`` - select desired module functionality for a Sonoff TH (Wait for the restart)
3. ``gpios`` - show supported sensor types. (DHT21 = AM2301, AM2302 = AM2321 = DHT22)
4. ``gpio`` - show current defined sensors on supported GPIO pins
5. ``gpio14 2`` - select sensor AM2301 (Wait for the restart)

For some sensors to show up a power cycle of Sonoff is needed to reset the devices just configured.

## I2C sensor
The following I2C sensors are supported using ``I2C SCL (5)`` and ``I2C SDA (6)`` in group 3:

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sensors1.jpg" width="230" align="right" />


- BH1750 Ambient Light Intensity
- BMP180 Pressure and Temperature in group 3a
- BMP280 Pressure and Temperature in group 3a
- BME280 Pressure, Temperature and Humidity in group 3a
- HTU21 Temperature and Humidity in group 3b
- SI7013 Temperature and Humidity in group 3b
- SI7020 Temperature and Humidity in group 3b
- SI7021 Temperature and Humidity in group 3b
- SHT10 Temperature and Humidity in group 3c
- SHT11 Temperature and Humidity in group 3c
- SHT15 Temperature and Humidity in group 3c

You can add I2C sensors using the following (MQTT) commands:

1. ``modules`` - show supported modules
2. ``module 1`` - select desired module functionality for a Sonoff Basic (Wait for the restart)
3. ``gpios`` - show supported sensor types. We need two pins: I2C SCL (5) and I2C SDA (6)
4. ``gpio`` - show current defined sensors on supported GPIO pins
5. ``gpio14 5`` - select I2C SCL (Wait for the restart)
6. ``gpio4 6`` - select I2C SDA (Wait for the restart)

The software will autodetect the connected I2C devices. For some sensors to show up a power cycle of Sonoff is needed to reset the devices just configured.

## Device
The following additional device(s) are supported:
- WS2812 led string - ``WS2812 (7)`` in group 4<br/>Using NeoPixelBus library and external 5V power supply
- IR Remote - ``IRremote (8)`` in group 5<br/>Using IRremoteESP8266 library and additional IR led hardware as documented [here](https://github.com/altelch/SonoffIR)

You can add a device using the following (MQTT) commands:

1. ``modules`` - show supported modules
2. ``module 1`` - select desired module functionality for a Sonoff Basic (Wait for the restart)
3. ``gpios`` - show supported sensor types. We need WS2812 (7)
4. ``gpio`` - show current defined sensors on supported GPIO pins
5. ``gpio14 7`` - select WS2812 led string (Wait for the restart)

For some devices a power cycle of Sonoff is needed to reset the interface to the devices just configured.
