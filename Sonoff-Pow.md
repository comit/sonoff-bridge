## Calibration
Sonoff Pow might need calibration as correct measurements are influenced by hardware and timing differences.

I used the following procedure to calibrate.

1. Prerequisites
    - Calibrated multimeter ;-)
    - Optional calibrated power meter ;-)
    - 60W light bulb
2. Connect the Pow to the optional power meter.
3. Connect the 60W light bulb
4. Open a webbrowser to Pow showing the main page and another webbrowser showing the Console
5. Turn power on and wait a few seconds for the pow to settle on a stable power reading
6. Verify the power reading with the power meter or expected 60W and if needed change the power offset value with command ```HLWPcal```
7. Verify the voltage reading with the multimeter and if needed change the voltage offset value with command ```HLWUcal```
8. Verify the current reading with the calculated value of P (step 6) / U (step 7) and if needed change the current offset with command ```HLWIcal```

## Result explanation
The Sonoff Pow can provide Energy, Power, Voltage and Current information in different ways.

### Push result using Telemetry
The preffered way is using the periodic telemetry data. Setting ```teleperiod 300``` will send telemetry data every 5 minutes.
```
tele/pow1/ENERGY = {"Time":"2017-02-15T11:16:20", "Yesterday":0.012, "Today":0.000, "Period":0, "Power":4, "Factor":0.35, "Voltage":230, "Current":0.050}
```

### Pull result using status message 8
To request information you can use command ```status 8```.
```
stat/pow1/STATUS8 = {"StatusPWR":{"Yesterday":0.012, "Today":0.000, "Power":4, "Factor":0.36, "Voltage":228, "Current":0.054}}
```

### Meaning
The presented information has the following meaning:
```
Message   | Unit | Description
----------|------|-----------------------------------------------------
Yesterday | kWh  | Total Energy usage between 00:00 and 24:00 yesterday
Today     | kWh  | Total Energy usage today from 00:00 until now
Period    | Wh   | Energy usage between previous message and now
Power     | W    | Current power load
Factor    |      | The ratio of the real power flowing to the load to
          |      |   the apparent power in the circuit 
Voltage   | V    | Current line voltage
Current   | A    | Current line current
```

## Debugging
Debugging the Sonoff Pow is a bit tricky as the serial interface has a **direct connection to one of the AC power lines**. Therefore I designed below schematic which allows for serial monitoring at 115200 baud and uploading of firmware up to 57600 baud while AC is connected.
<img alt="OptoSerial" src="https://github.com/arendst/arendst.github.io/blob/master/media/OptoSerial.jpg" /> 
