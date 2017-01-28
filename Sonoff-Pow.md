## Calibration
Sonoff Pow might need calibration as correct measurements are influenced by hardware and timing differences.

I used the following procedure to calibrate.

1. Prerequisites
    - Calibrated multimeter ;-)
    - Optional calibrated power meter ;-)
    - 60W light bulb
2. Enable the calibration commands by updating define ```USE_POWERCALIBRATION``` in ```user_config.h```
3. Recompile and upload to Sonoff Pow
4. Connect the Pow to the optional power meter.
5. Connect the 60W light bulb
6. Open a webbrowser to Pow showing the main page and another webbrowser showing the Console
7. Turn power on and wait a few seconds for the pow to settle on a stable power reading
8. Verify the power reading with the power meter or expected 60W and if needed change the power offset value with command ```HLWPcal```
9. Verify the voltage reading with the multimeter and if needed change the voltage offset value with command ```HLWUcal```
10. Verify the current reading with the calculated value of P (step 8) / U (step 9) and if needed change the current offset with command ```HLWIcal```
11. Optionally disable the calibration commands by recompiling and uploading the firmware

## Result explanation
The Sonoff Pow can provide Energy, Power, Voltage and Current information in different ways.

### Push result using Telemetry
The preffered way is using the periodic telemetry data. Setting ```teleperiod 300``` will send telemetry data every 5 minutes. Depending on messageformat and mqttunits the result would be:

- Result when ```messageformat 0``` (Legacy) and ```mqttunits 0```: 
```
tele/pow1/YESTERDAY_ENERGY 0.234
tele/pow1/TODAY_ENERGY 0.016
tele/pow1/PERIOD_ENERGY 5
tele/pow1/CURRENT_POWER 53
tele/pow1/POWER_FACTOR 1.00
tele/pow1/VOLTAGE 214
tele/pow1/CURRENT 0.247
tele/pow1/TIME 2017-01-01T13:50:17
```
- Result when ```messageformat 0``` (Legacy) and ```mqttunits 1```:
```
tele/pow1/YESTERDAY_ENERGY 0.234 kWh
tele/pow1/TODAY_ENERGY 0.016 kWh
tele/pow1/PERIOD_ENERGY 5 Wh
tele/pow1/CURRENT_POWER 53 W
tele/pow1/POWER_FACTOR 1.00
tele/pow1/VOLTAGE 214 V
tele/pow1/CURRENT 0.247 A
tele/pow1/TIME 2017-01-01T13:50:17
```
- Result when ```messageformat 1``` (JSON):
```
tele/pow1/TELEMETRY {"Time":"2017-01-01T13:50:17", "Energy":{"Yesterday":"0.234", "Today":"0.016", "Period":5, "Power":53, "Factor":"1.00", "Voltage":214, "Current":"0.247"}}
```

### Pull result using status message 8
To request information you can use command ```status 8``` which results in one of the following messages:

- Result when ```messageformat 0``` (Legacy):
```
stat/pow1/RESULT PWR: Voltage 214 V, Current 0.247 A, Power 53 W, Today 0.016 kWh, Factor 1.00
```
- Result when ```messageformat 1``` (JSON):
```
stat/pow1/RESULT {"StatusPWR":{"Voltage":214, "Current":"0.247", "Power":53, "Today":"0.016", "Factor":"1.00"}}
```

### Meaning
The presented information has the following meaning:

Legacy | JSON | Unit | Description
------ | ---- | ---- | -----------
YESTERDAY_ENERGY | Yesterday | kWh | Total Energy usage between 00:00 and 24:00 yesterday
TODAY_ENERGY | Today | kWh | Total Energy usage today from 00:00 until now
PERIOD_ENERGY | Period | Wh | Energy usage between previous message and now
CURRENT_POWER | Power | W | Current power load
VOLTAGE | Voltage | V | Current line voltage
CURRENT | Current | A | Current line current
POWER_FACTOR | Factor |  | The ratio of the real power flowing to the load to the apparent power in the circuit 
