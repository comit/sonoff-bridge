[Home Assistant](https://home-assistant.io/) (HA) is an open-source home automation platform running on Python 3. Track and control all devices at home and automate control. 

Configure HA for Sonoff-TASMOTA by editing the file ``configuration.yaml`` to be found in folder ``.homeassistant`` after first installation.

After every change to the configuration file you'll need to restart HA to make it aware of the changes. On my Debian Linux system I perform the command ``systemctl restart home-assistant``.

In the examples shown the following Sonoff-TASMOTA parameters are set:
- ``MQTT_STATUS_OFF`` in ``user_config.h`` = ``OFF``. This is the default state
- ``MQTT_STATUS_ON`` in ``user_config.h`` = ``ON``. This is the default state
- ``SUB_PREFIX`` in ``user_config.h`` = ``cmnd``
- ``PUB_PREFIX`` in ``user_config.h`` = ``stat``
- ``PUB_PREFIX2`` in ``user_config.h`` = ``tele``
- ``Mqtt`` = 1
- ``MqttHost`` = ``domus1``
- ``MqttPort`` = 1883
- ``Topic`` = ``sonoff``
- ``PowerRetain`` = 1

## Add MQTT broker

As Sonoff-TASMOTA is MQTT based you will need to configure the MQTT Broker used by TASMOTA in HA. Update your HA configuration file with the local MQTT server ``domus1``.
```
mqtt:
  broker: domus1
  port: 1883
  client_id: home-assistant-1
  keepalive: 60
  username: HAUSERNAME1
  password: HAPASSWORD1
  protocol: 3.1
  birth_message:
    topic: 'tele/hass1/LWT'
    payload: 'Online'
    qos: 1
    retain: true
  will_message:
    topic: 'tele/hass1/LWT'
    payload: 'Offline'
    qos: 1
    retain: true
```

## Basic functionality

As HA is non persistent it is important to configure TASMOTA for sending retained power status messages to the broker. This is accomplished with the TASMOTA command ``PowerRetain On`` or ``cmnd/sonoff/PowerRetain On``.

Add the device as a switch to HA by updating the configuration file.
```
switch:
  - platform: mqtt
    name: "Sonoff power"
    state_topic: "stat/sonoff/POWER"
    command_topic: "cmnd/sonoff/POWER"
    qos: 1
    payload_on: "ON"
    payload_off: "OFF"
    retain: true
```

## Example using DHT22 sensor

A working Home Assistant example using a DHT22 sensor on a Sonoff TH10

The output from the Sonoff TH10 ``Status 10`` command looks like this:
```
stat/sonoff/STATUS10 {"StatusSNS":{"Time":"2017-02-11T18:06:05", "DHT":{"Temperature":"21.8", "Humidity":"48.0"}}}
```

The Home Assistant configuration addition would be as follows:
```
sensor:
  - platform: mqtt
    state_topic: 'stat/sonoff/STATUS10'
    command_topic: 'cmnd/sonoff/status'
    payload: '10'
    name: 'Temperature'
    unit_of_measurement: '°C'
    value_template: '{{ value_json.StatusSNS.DHT.Temperature }}'
  - platform: mqtt
    state_topic: 'stat/sonoff/STATUS10'
    name: 'Humidity'
    unit_of_measurement: '%'
    value_template: '{{ value_json.StatusSNS.DHT.Humidity }}'
  - platform: mqtt
    state_topic: 'tele/sonoff/SENSOR'
    name: 'Temperature'
    unit_of_measurement: '°C'
    value_template: '{{ value_json.DHT.Temperature }}'

```
The first two will pick up the temp and humidity on the DHT22 sensor output when there is a mqtt pub request like this ``mosquitto_pub -h localhost -t 'cmnd/sonoff/status' -m '10'``

The last sensor picks up what the SONOFF and this software sends periodically and is done automatically at this point. This periodic interval can be changed using the ``TelePeriod`` command (see the wiki or the MQTT commands).
