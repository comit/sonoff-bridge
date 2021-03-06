## Sonoff-Tasmota
<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffbasic.jpg" width="250" align="right" />

Provide ESP8266 based Sonoff by [iTead Studio](https://www.itead.cc/) and ElectroDragon IoT Relay with Serial, Web and MQTT control allowing 'Over the Air' or OTA firmware updates using Arduino IDE.

## Supported Devices
The following devices are supported with Serial, Web and MQTT control:
- [iTead Sonoff Basic](http://sonoff.itead.cc/en/products/sonoff/sonoff-basic)<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff_th.jpg" width="250" align="right" /> 
- [iTead Sonoff RF](http://sonoff.itead.cc/en/products/sonoff/sonoff-rf)
- [iTead Sonoff SV](https://www.itead.cc/sonoff-sv.html)
- [iTead Sonoff TH10/TH16 with temperature sensor](http://sonoff.itead.cc/en/products/sonoff/sonoff-th)
- [iTead Sonoff Dual (R2)](http://sonoff.itead.cc/en/products/sonoff/sonoff-dual)
- [iTead Sonoff Pow](http://sonoff.itead.cc/en/products/sonoff/sonoff-pow)
- [iTead Sonoff 4CH](http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch)
- [iTead Sonoff 4CH Pro](http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch-pro)
- [iTead S20 Smart Socket](http://sonoff.itead.cc/en/products/residential/s20-socket)
- [Sonoff S22 Smart Socket](https://github.com/arendst/Sonoff-Tasmota/issues/627)
- [iTead Slampher](http://sonoff.itead.cc/en/products/residential/slampher-rf)
- [iTead Sonoff Touch](http://sonoff.itead.cc/en/products/residential/sonoff-touch)
- [iTead Sonoff T1](http://sonoff.itead.cc/en/products/residential/sonoff-t1)
- [iTead Sonoff SC](http://sonoff.itead.cc/en/products/residential/sonoff-sc)
- [iTead Sonoff Led](http://sonoff.itead.cc/en/products/appliances/sonoff-led)
- [iTead Sonoff BN-SZ01 Ceiling Led](http://sonoff.itead.cc/en/products/appliances/bn-sz01)
- [iTead Sonoff B1](http://sonoff.itead.cc/en/products/residential/sonoff-b1)
- [iTead Sonoff RF Bridge 433](http://sonoff.itead.cc/en/products/appliances/sonoff-rf-bridge-433)
- [iTead Sonoff Dev](https://www.itead.cc/sonoff-dev.html)
- [iTead 1 Channel Switch 5V / 12V](https://www.itead.cc/smart-home/inching-self-locking-wifi-wireless-switch.html)
- [iTead Motor Clockwise/Anticlockwise](https://www.itead.cc/smart-home/motor-reversing-wifi-wireless-switch.html)
- [Electrodragon IoT Relay Board](http://www.electrodragon.com/product/wifi-iot-relay-board-based-esp8266/)
- AI Light or any my9291 compatible RGBW LED bulb
- H801 PWM LED controller
- [MagicHome PWM LED controller (aka Flux-Light LED module)](MagicHome-LED-strip-controller)
- [AriLux AL-LC01, AL-LC06 and AL-LC11 PWM LED controller](https://www.banggood.com/ARILUX-AL-LC01-Super-Mini-LED-WIFI-Smart-RGB-Controller-For-RGB-LED-Strip-Light-DC-9-12V-p-1058603.html)
- [Supla device - Espablo-inCan mod. for electrical Installation box](https://forum.supla.org/viewtopic.php?f=33&t=2188)
- [Luani HVIO board](https://luani.de/projekte/esp8266-hvio/)
- Wemos D1 mini and NodeMcu

## Features
<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff4ch.jpg" width="250" align="right" />
The following features are available:

- Multiple devices can be addressed by MQTT `GroupTopic`
- Firmware upload by OTA or via web page upload
- Expanded Status messages
- UDP syslog messages can be filtered on program name starting with `ESP-`
- The button can send a different MQTT message defined with `ButtonTopic`
- Initial Wifi setup by user_config.h, Serial, Smartconfig, Wifi manager or WPS config
- A web server provides control of Sonoff and contains a firmware upload facility
- Support for DHTxx, AM2301 or DS18B20 temperature sensors as used in Sonoff TH10/TH16
- Support for I2C sensors BH1750, BME280, BMP280, HTU21, SI70xx and SHT1x
- Telemetry data can be send using optional different prefix from status messages
- Native Domoticz MQTT support
- Easy integration in home automation solutions like openHAB, HomeAssistant, ...
- Wemo and Hue emulation for Amazon Echo (Alexa) support

## Community
See [Community](https://groups.google.com/d/forum/sonoffusers) for forum and additional user experience
