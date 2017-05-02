The main button on a Sonoff-Tasmota flashed module provides the following functionalities:

- **1 short press:** Toggles the relay either directly or by sending a MQTT message like ```cmnd/sonoff/1/light on```. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/LIGHT on```. If ```cmnd/sonoff/ButtonRetain on``` has been used the MQTT message will also contain the MQTT retain flag.

- **2 short presses:** Toggles the relay. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/POWER on```. For Sonoff Dual this will switch relay 2.

- **3 short presses:** Start Wifi smartconfig allowing for SSID and Password configuration using an Android mobile phone with the [ESP8266 SmartConfig](https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch) app. The MQTT server still needs to be configured in the ```user_config.h``` file. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.

- **4 short presses:** Start Wifi manager providing an Access Point with IP address 192.168.4.1 and a web server allowing the configuration of both Wifi and MQTT parameters. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.

- **5 short presses:** Start Wifi Protected Setup (WPS) allowing for SSID and Password configuration using the routers WPS button or webpage. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.

- **6 short presses:** Will restart the module

- **7 short presses:** Start OTA download of firmware. The green LED is lit during the update

- **Pressing the button for over four seconds:** Reset settings to defaults as defined in ```user_config.h``` and restarts the device

The button is connected to GPIO 0 (NodeMCU pin D3)
