## Add MQTT broker used by Sonoff

To configure your MQTT broker called ``domus1`` in Home Assistant you'll need to add the following lines to your ``configuration.yaml`` file:
```
# MQTT server
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

## Example using DHT22 sensor

A working Home Assistant example using a DHT22 sensor on a Sonoff TH10

The output from the Sonoff TH10 ``Status 10`` command looks like this:
```
stat/sonoff/STATUS10 {"StatusSNS":{"Time":"2017-02-11T18:06:05", "DHT":{"Temperature":"21.8", "Humidity":"48.0"}}}
```

The Home Assistant configuration could be as follows:
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