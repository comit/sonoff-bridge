(This information is related to Sonoff-Tasmota version 3.9.11 and up)

[Home Assistant](https://home-assistant.io/) (HA) is an open-source home automation platform running on Python 3.

Configure HA by editing the file ``configuration.yaml`` to be found in folder ``.homeassistant`` at first installation.

After every change to the configuration file you'll need to restart HA to make it aware of the changes. On my Debian Linux system I perform the command ``systemctl restart home-assistant``.

In the examples shown the Sonoff-Tasmota parameters are set:
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

## MQTT broker

As Sonoff-Tasmota is MQTT based you will need to configure Home Assistant to [connect to an MQTT broker](https://home-assistant.io/components/mqtt/). You can use the following configuration to an MQTT server with the hostname ``domus1``.
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

## Switch

As HA is non persistent it is important to configure Sonoff-Tasmota for sending retained power status messages to the broker. This is accomplished with the command ``PowerRetain On`` or ``cmnd/sonoff/PowerRetain On``.

Add the device as a switch to HA by updating the configuration file.
```
switch:
  - platform: mqtt
    name: "Sonoff power"
    state_topic: "stat/sonoff/POWER"
    command_topic: "cmnd/sonoff/POWER"
    availability_topic: "tele/sonoff/LWT"
    qos: 1
    payload_on: "ON"
    payload_off: "OFF"
    payload_available: "Online"
    payload_not_available: "Offline"
    retain: true
```
If you are using your Sonoff to control a light, you may want to use the `light` component. Simply replace `switch` with `light` in the above configuration. All other settings remain the same.

Further documentation on the Home Assistant `switch` and `light` components can be found here:

https://home-assistant.io/components/switch.mqtt/

https://home-assistant.io/components/light.mqtt/

## DHT22 sensor

### Periodical updates

A DHT22 Temperature and Humidity sensor connected to a Sonoff TH10 will send at ``TelePeriod`` intervals the following information to the MQTT broker:
```
tele/sonoff/SENSOR = {"Time":"2017-02-12T16:11:12", "DHT22":{"Temperature":23.9, "Humidity":34.1}}
```
To make the information visible in HA add the following lines to the configuration file.
```
sensor:
  - platform: mqtt
    name: "Tele Temperature"
    state_topic: "tele/sonoff/SENSOR"
    value_template: "{{ value_json['DHT22'].Temperature }}"
    unit_of_measurement: "°C"
  - platform: mqtt
    name: "Tele Humidity"
    state_topic: "tele/sonoff/SENSOR"
    value_template: "{{ value_json['DHT22'].Humidity }}"
    unit_of_measurement: "%"
```
This periodic interval can be changed using the ``TelePeriod`` command (see the wiki for the MQTT commands).

### Manual updates

Another means of sensor information retrieval from Sonoff-Tasmota is using the status command ``Status 10`` or ``cmnd/sonoff/status 10``. This would result in a message like:
```
stat/sonoff/STATUS10 {"StatusSNS":{"Time":"2017-02-11T18:06:05", "DHT22":{"Temperature":"21.8", "Humidity":"48.0"}}}
```
The HA configuration would then look like this:
```
sensor:
  - platform: mqtt
    name: "Stat Temperature"
    state_topic: "stat/sonoff/STATUS10"
    value_template: "{{ value_json.StatusSNS.DHT22.Temperature }}"
    unit_of_measurement: "°C"
  - platform: mqtt
    name: "Stat Humidity"
    state_topic: "stat/sonoff/STATUS10"
    value_template: "{{ value_json.StatusSNS.DHT22.Humidity }}"
    unit_of_measurement: "%"
```
The Sonoff-Tasmota command could be initiated by a mosquitto mqtt pub command on ``mosquitto_pub -h localhost -t 'cmnd/sonoff/status' -m '10'``

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
The HA configuration for Energy, Power and Voltage would be:
```
sensor:
  - platform: mqtt
    name: "Energy"
    state_topic: "tele/pow1/ENERGY"
    value_template: "{{ value_json.Today }}"
    unit_of_measurement: "kWh"
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