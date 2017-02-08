The software supports:
- [single wire sensors](#single-wire-sensor)
- [dual wire or I2C sensors](#i2c-sensor)
- [single wire devices](#devices)

## Single wire sensor
The following single wire sensors are supported:
- ``DHT11 (1)`` - DHT11 Temperature and Humidity
- ``AM2301 (2)`` - DHT21 Temperature and Humidity
- ``AM2301 (2)`` - AM2301 Temperature and Humidity
- ``DHT22 (3)`` - DHT22 Temperature and Humidity
- ``DHT22 (3)`` - AM2302 Temperature and Humidity
- ``DHT22 (3)`` - AM2321 Temperature and Humidity
- ``DS18x20 (4)`` - DS18B20 Temperature (multiple sensors with optional OneWire library enabled in ``user_config.h``)
- ``DS18x20 (4)`` - DS18S20 Temperature (with optional OneWire library enabled in ``user_config.h``)
- ``Switch (8)`` - External switch
- ``Switch (8)`` - HC-SR501 PIR Motion Detection (configured as Switch)

You can dynamically add a sensor using the following (MQTT) commands:

1. ``modules`` - show supported modules
2. ``module 4`` - select desired module functionality for a Sonoff TH (Wait for the restart)
3. ``gpios`` - show supported sensor types. (DHT21 = AM2301, AM2302 = AM2321 = DHT22)
4. ``gpio`` - show current defined sensors on supported GPIO pins
5. ``gpio14 2`` - select sensor AM2301 (Wait for the restart)

For some sensors to show up a power cycle of Sonoff is needed to reset the devices just configured.

## I2C sensor
The following I2C sensors are supported using ``I2C SCL (5)`` and ``I2C SDA (6)``:
- BH1750 Ambient Light Intensity
- BMP180 Pressure and Temperature
- BMP280 Pressure and Temperature
- BME280 Pressure, Temperature and Humidity
- HTU21  Temperature and Humidity

You can dynamically add I2C sensors using the following (MQTT) commands:

1. ``modules`` - show supported modules
2. ``module 1`` - select desired module functionality for a Sonoff Basic (Wait for the restart)
3. ``gpios`` - show supported sensor types. We need two pins: I2C SCL (5) and I2C SDA (6)
4. ``gpio`` - show current defined sensors on supported GPIO pins
5. ``gpio14 5`` - select I2C SCL (Wait for the restart)
6. ``gpio4 6`` - select I2C SDA (Wait for the restart)

The software will autodetect the connected I2C devices. For some sensors to show up a power cycle of Sonoff is needed to reset the devices just configured.

## Device
The following additional device(s) are supported:
- ``WS2812 (7)`` - WS2812 led string using NeoPixelBus library and external 5V power supply

You can dynamically add a device using the following (MQTT) commands:

1. ``modules`` - show supported modules
2. ``module 1`` - select desired module functionality for a Sonoff Basic (Wait for the restart)
3. ``gpios`` - show supported sensor types. We need WS2812 (7)
4. ``gpio`` - show current defined sensors on supported GPIO pins
5. ``gpio14 7`` - select WS2812 led string (Wait for the restart)

For some devices a power cycle of Sonoff is needed to reset the interface to the devices just configured.
