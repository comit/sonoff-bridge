[Home Assistant](https://home-assistant.io/) (HA) is an open-source home automation platform running on Python 3.

Configure HA by editing the file ``configuration.yaml`` to be found in folder ``.homeassistant`` at first installation.

After every change to the configuration file you'll need to restart HA to make it aware of the changes. On my Debian Linux system I perform the command ``systemctl restart home-assistant``.

In the examples shown the Sonoff-TASMOTA parameters are set:
- ``MQTT_STATUS_OFF`` in ``user_config.h`` = ``OFF``
- ``MQTT_STATUS_ON`` in ``user_config.h`` = ``ON``
- ``SUB_PREFIX`` in ``user_config.h`` = ``cmnd``
- ``PUB_PREFIX`` in ``user_config.h`` = ``stat``
- ``PUB_PREFIX2`` in ``user_config.h`` = ``tele``
- ``Mqtt`` = 1
- ``MqttHost`` = ``domus1``
- ``MqttPort`` = 1883
- ``Topic`` = ``sonoff``
- ``PowerRetain`` = 1
- ``TelePeriod`` = 300

The messages shown are valid for Sonoff-TASMOTA version 3.9.15 and up. 

## Add MQTT broker

As Sonoff-TASMOTA is MQTT based you will need to configure the MQTT Broker in HA. Update your HA configuration file with the local MQTT server ``domus1``.
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
    topic: "tele/hass1/LWT"
    payload: "Online"
    qos: 1
    retain: true
  will_message:
    topic: "tele/hass1/LWT"
    payload: "Offline"
    qos: 1
    retain: true
```

## Basic functionality

As HA is non persistent it is important to configure Sonoff-TASMOTA for sending retained power status messages to the broker. This is accomplished with the command ``PowerRetain On`` or ``cmnd/sonoff/PowerRetain On``.

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

## DHT22 sensor

### Periodical updates

A DHT22 Temperature and Humidity sensor connected to a Sonoff TH10 will send at ``TelePeriod`` intervals the following information to the MQTT broker:
```
tele/sonoff/SENSOR = {"Time":"2017-02-12T16:11:12", "DHT":{"Temperature":23.9, "Humidity":34.1})
```
To make the information visible in HA add the following lines to the configuration file.
```
sensor:
  - platform: mqtt
    name: "Tele Temperature"
    state_topic: "tele/sonoff/SENSOR"
    value_template: "{{ value_json.DHT.Temperature }}"
    unit_of_measurement: "°C"
  - platform: mqtt
    name: "Tele Humidity"
    state_topic: "tele/sonoff/SENSOR"
    value_template: "{{ value_json.HTU21.Humidity }}"
    unit_of_measurement: "%"
```
This periodic interval can be changed using the ``TelePeriod`` command (see the wiki for the MQTT commands).

### Manual updates

Another means of sensor information retrieval from Sonoff-TASMOTA is using the status command ``Status 10`` or ``cmnd/sonoff/status 10``. This would result in a message like:
```
stat/sonoff/STATUS10 {"StatusSNS":{"Time":"2017-02-11T18:06:05", "DHT":{"Temperature":"21.8", "Humidity":"48.0"}}}
```
The HA configuration would then look like this:
```
sensor:
  - platform: mqtt
    name: "Stat Temperature"
    state_topic: "stat/sonoff/STATUS10"
    value_template: "{{ value_json.StatusSNS.DHT.Temperature }}"
    unit_of_measurement: "°C"
  - platform: mqtt
    name: "Stat Humidity"
    state_topic: "stat/sonoff/STATUS10"
    value_template: "{{ value_json.StatusSNS.DHT.Humidity }}"
    unit_of_measurement: "%"
```
The Sonoff-TASMOTA command could be initiated by a mosquitto mqtt pub command on ``mosquitto_pub -h localhost -t 'cmnd/sonoff/status' -m '10'``

## HTU and BMP I2C sensors

HTU21 and BMP280 sensors connected to ``sonoff2`` send messages like:
```
16:16:43 MQTT: tele/sonoff2/SENSOR = {"Time":"2017-02-12T16:16:43", "HTU21":{"Temperature":24.0, "Humidity":34.0}, "BMP280":{"Temperature":24.9, "Pressure":1032.5}}
```
Where the Pressure information would be made available to HA with
```
sensor:
  - platform: mqtt
    name: "Tele Pressure"
    state_topic: "tele/sonoff2/SENSOR"
    value_template: "{{ value_json.BMP280.Pressure }}"
    unit_of_measurement: "hPa"
```

## Sonoff Pow Energy sensors

### Periodical updates

A Sonoff Pow device called ``pow1`` will periodically send the following message:
```
tele/pow1/ENERGY = {"Time":"2017-02-12T16:35:00", "Yesterday":0.002, "Today":0.001, "Period":0, "Power":4, "Factor":0.32, "Voltage":215, "Current":0.060}
```
The HA configuration for Power and Voltage would be:
```
sensor:
  - platform: mqtt
    name: "Power"
    state_topic: "tele/pow1/ENERGY"
    value_template: "{{ value_json.Power }}"
    unit_of_measurement: "W"
  - platform: mqtt
    name: "Voltage"
    state_topic: "tele/pow1/ENERGY"
    value_template: "{{ value_json.Voltage }}"
    unit_of_measurement: "V"
```

### Manual updates

The manual message retrieved with command ``Status 8`` or ``cmnd/pow1/status 8`` will show:
```
stat/pow1/STATUS8 = {"StatusPWR":{"Yesterday":0.002, "Today":0.002, "Power":4, "Factor":0.37, "Voltage":227, "Current":0.056}}
```
The HA configuration for Power Factor would then be:
```
sensor:
  - platform: mqtt
    name: "Power Factor"
    state_topic: "stat/pow1/STATUS8"
    value_template: "{{ value_json.StatusPWR.Factor }}"
```