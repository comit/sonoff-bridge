<img alt="Sonoff" src="https://github.com/arendst/arendst.github.io/blob/master/media/domoticz2.jpg" width="250" align="right" /> 
Sonoff supports [Domoticz](http://www.domoticz.com/) MQTT 'out of the box'. Find below the procedure to start using it.

## Assumptions
The following servers should be made available:

- You have installed/access to a MQTT server and made contact with your sonoff
- You have installed Domoticz

## Domoticz
Configure MQTT and Virtual Sensor hardware

1. On the hardware page add Type ```MQTT Client Gateway with LAN interface```
    1. Give it a name
    2. Configure the interface with access to your MQTT server (```Remote Address```, ```Port```, ```Username``` and ```Password```)
    3. Set the ```Public Topic``` to flat (which seems to relate to ```out``` in the select field)
2. On the hardware page add Type ```Dummy (used for virtual switches)```
    1. Give it a name
3. Make a new virtual switch to be used with Sonoff by clicking ```Create Virtual Sensors```
    1. Give it a name
    2. Select ```Sensor Type Switch```
4. On the Devices page find the new switch by it's name
    1. Remember it's Idx number

## Sonoff
<img alt="Sonoff" src="https://github.com/arendst/arendst.github.io/blob/master/media/domoticz3.jpg" width="250" align="right" /> 
Sonoff provides several ways of configuring Domoticz parameters.

- Use the webinterface and select ```Configuration - Configure Domoticz```
    1. Set ```In topic``` to ```domoticz/in``` as hardcoded in Domoticz
    2. Set ```Out topic``` to ```domoticz/out``` as hardcoded in Domoticz
    3. Configure ```Idx 1``` to the value read in step 4.1
- Use MQTT and execute commands
    1. ```cmnd/sonoff/DomoticzInTopic``` with payload ```domoticz/in``` as hardcoded in Domoticz
    2. ```cmnd/sonoff/DomoticzOutTopic``` with payload ```domoticz/out``` as hardcoded in Domoticz
    3. ```cmnd/sonoff/DomoticzIdx1``` with payload value read in step 4.1
- Use the serial interface and execute commands
    1. ```DomoticzInTopic``` with ```domoticz/in``` as hardcoded in Domoticz
    2. ```DomoticzOutTopic``` with ```domoticz/out``` as hardcoded in Domoticz
    3. ```DomoticzIdx1``` with the value read in step 4.1

## Domoticz    
That's it! You can now switch Sonoff from the Domoticz user interface.

- On the Switches page scroll down and find your Switch as configured in step 3
    - Toggle the light bulb; Sonoff should respond