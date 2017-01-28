Install the ESP8266 Arduino development environment from [esp8266 Arduino](https://github.com/esp8266/Arduino). The software is supported with Arduino IDE versions 1.6.10 until 1.8.1 and esp8266 Arduino stable version 2.3.0.

- I prefer a standalone version of the IDE allowing easy ESP8266 file manipulation. This can be achieved by downloading the Arduino IDE ZIP file for non admin install. After unzipping and before executing ```arduino.exe``` add an empty directory called ```portable```
- Follow the procedure from the ESP8266 Arduino README.md to install the development environment using the Arduino IDE Board Manager
- Copy the ```sonoff``` directory to your sketchfolder

Download and unzip the [pubsubclient](https://github.com/knolleary/pubsubclient) MQTT library into directory ```portable\sketchbook\libraries``` and rename to pubsubclient. Update default value in file ```pubsubclient\src\PubSubClient.h```  
- Change ```MQTT_MAX_PACKET_SIZE``` from 128 to at least 400  

Optionally install php and a local web server (ie apache) for OTA and copy directory ```api``` in webroot.

### "Over The Air" updates
If you want to be able to upload the OTA file from the IDE to your web server perform the following changes to the Arduino IDE environment:

- Copy file ```espupload.py``` to directory ```portable\packages\esp8266\hardware\esp8266\2.3.0\tools``` and change HOST_ADDR to refer to your web server
- Replace files ```boards.txt``` and ```platform.txt``` in directory ```portable\packages\esp8266\hardware\esp8266\2.3.0```

This will provide an additional option ```Tools - Upload Using: OTA_upload```.
