You've successfully uploaded the Sonoff-Tasmota firmware to your module.

1. Connect to (AC) power (Serial NOT connected)
2. Configure Wifi by using one of the provided configuration modes: [Button Usage](Button-Usage)
3. Connect to the device webpage
4. Select the correct Sonoff module type from the configuration frontend
5. Configure the MQTT broker connection, choose a unique topic, e.g. "sonoff-kitchenlight"
6. Test communication with the MQTT client of your choice
7. Integrate with the Control interface or home automation solution of your choice

## Add a sensor
You can dynamically add a sensor using the following (MQTT) commands:

- ``modules`` - show supported modules
- ``module 4`` - select desired module functionality for a Sonoff TH (Wait for the restart)
- ``gpios`` - show supported sensor types. (DHT21 = AM2301, AM2302 = AM2321 = DHT22)
- ``gpio`` - show current defined sensors on supported GPIO pins
- ``gpio14 2`` - select sensor AM2301 (Wait for the restart)

For some sensors to show up a power cycle of Sonoff is needed to reset the devices just configured


