A possible scenario is the conjunction of the above services. The installation and basic knowledge of the required software packages is out this scope here, but there are lots of good explanations to be found on the web. The installation of the „middleware“ on one tiny computer, for instance a Pi-like device, is recommended and tested, but you can distribute the services as you like.

Necessary steps:

1. Install HOMEBRIDGE and install HOMEBRIDGE-MQTT as your only plugin. You can achieve everything (actors and sensors) with this single plugin.
2. Install Node-RED in the recommended way for your device. Nothing special here.
3. Install a MQTT-Broker for instance MOSQUITTO. Again nothing special here.
4. Make sure, your TASMOTA-device is visible und works as expected with MQTT. Basic checking is possible in the web-console via MQTT-Commands (for example: cmnd/sonoff/POWER 1). For the connection to the MQTT-Broker (for instance MOSQUITTO) you need the network address and the port number.

In the example configuration Homebridge, Homebridge-mqtt, Node-RED and mosquitto all run on a single BANANA-PI with ARMBIAN. That way you only need to remember that one network address and you can connect Node-RED to MOSQUITTO via localhost or 127.0.0.1. 

The remaining steps are done in node.red, so you have to open the network-adress (of your node-red-installation) with portnumber (usually 1880).

Example:
TASMOTA-Device: H801 with a connected RGB-Strip, mqtt-name: H801.
Homebridge, Homebridge-mqtt, Node-RED and mosquitto: on 192.168.0.130


1. Inject a device in HomeKit:
    1. Use a inject node from the left side and select JSON for payload with the following payload:  {"name":"RGB-Strip","servicename":"Strip","service":"Lightbulb","Brightness":50,"Hue":50,"Saturation":100}
    2.  Then connect it to a mqtt-output-node and set the topic to: homebridge/to/add 
2. (optional) Make it possible, to remove the same device:
    1. Use a inject node from the left side and select JSON for payload with the following payload:  {"name":"RGB-Strip"}
    2.  Then connect it to a mqtt-output-node and set the topic to: homebridge/to/remove

You can test the device in the HomeKit-App of your choice. Without any work, we have a fully working GUI with color picker, brightness control and programmability. But of course there is no functionality yet. Now comes the real work.

3.  Connection from Homebridge to the H801 (running TASMOTA)
    1. Use a mqtt-input-node withe the topic „homebridge/from/set“, wire it to a json-function-node, the wire to a switch mode, which functions as filter.
    2. In this node set property to „msg.payload.name“ and down below „contains“ and „RGB-Strip“. Then wire to the next node, which is a generic function node.
    3. Now comes some Java-Script, which basically looks for the Brightness, Hue, Saturation and the On-Status of the Homekit-Device. It converts HSV to RGB via the COLORSYS, than converts RGB to PWM-values and returns these values for Red, Green and Blue as messages at the end.
    4. You then wire the R, G and B to mqtt-ouput-node with the payload cmnd/h801/PWM5, cmnd/h801/PWM4 and cmnd/h801/PWM3.
    5. ######## missing parts   #####!! (colorsys-plugin, global vars, flow-source-code)


That’s basically it. A „simple“ node-red-flow, which you can extend and tune. If you want to use that principle for other actors or sensors, there is a lot of copy-and-paste and in the future you don’t have to reinvent the wheel. The initial work is higher than to use a dedicated homebridge-plugin, but it is extremely flexible and you don’t have to touch your homebridge-config.json for a new device. So it is close to impossible to mess up a homebridge-installation.




