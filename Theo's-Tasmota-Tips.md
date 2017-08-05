### 20170714 - Tasmota needs compile Flash Mode option DOUT on all devices

An increasing number of devices is using the ESP8285. As this chip only supports a subset of hardware connections to it's inbuilt 1MB flash the firmware needs to be compiled with Flash Mode option set for DOUT.

This is documented in the wiki and the platformio.ini file.

As this compile option works for the ESP8266 "legacy" chip too I only provide firmware compiled with Flash Mode DOUT which runs fine on all released hardware like Sonoff, Wemos etc. See also [#598](https://github.com/arendst/Sonoff-Tasmota/issues/598).

Indications of selecting the wrong Flash Mode compile option (like DIO or QIO) on ESP8285 is a dead device while uploading just went fine [#683](https://github.com/arendst/Sonoff-Tasmota/issues/683).

### 20170425 - Tasmota version 5.x and up need linker script 1M with No SPIFFS

Starting with version 5.x a new linker script is made available allowing for 32k more code space.

See wiki how to implement it in your Arduino or platformIO IDE.
