This is for the lazy people that don't want to open the device, don't want to install SDKs and don't want to compile stuff ;-). You need
* A standard WiFi
* A different WiFi (which I created with a smartphone hotspot)
* A PC or similar with python3 that can run SonOTA

It worked for me with these devices:
* TH10
* TH16
* POW
* T1 (use the left button to set the device to config mode)

Steps:
* configure Sonoff using ewlink for local wifi and update the firmware. It does not work with old firmware.
* create wifi hotspot on smartphone for default indebuurt1/VnsqrtnrsddbrN
* on PC, extract sonota.py somwhere from the SonOTA project
* extract http://cputoasters.com/ameyer/sonoff/ssl/sonota-e2a.zip in same dir, should create ssl and static dir (contains just a snakeoil ssl cert and the default compiled e2a binaries directly from the unmodified Espressif2Arduino source)
* start sonota.py with your standard wifi SSID/PW and PC IP (eg ./sonota.py --wifi-ssid asdfasdf --wifi-password fidjfidfjidjf 192.168.123.123)
* connect Sonoff, press button for 5s or so until it blinks in blocks of 3, then again 5s or so until it blinks continuously
* connect to ITEAD-xxxx with PC
* sonota should send some stuff
* switch PC to standard Wifi
* sonota should send more stuff
* wait for a while, eg 2min, while the device is connecting to the standard wifi and downloading e2a (second part) / Tasmota
* connect the pc to the hotspot indebuurt1 wifi
* use `nmap 192.168.43.1-255 -p 80` or similar to find the IP of Tasmota minimal running on the device
* use a web browser to connect, update to Tasmota standard and configure WiFi / MQTT etc

If there is a problem with hosting the compiled binaries of the unmodified e2a source, just tell me and I will take it down.