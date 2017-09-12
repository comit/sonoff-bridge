
* Itead Product Page: http://sonoff.itead.cc/en/products/sonoff/sonoff-pow
* Itead Shop: https://www.itead.cc/sonoff-pow.html
* Itead Wiki: https://www.itead.cc/wiki/Sonoff_Pow

## ⚠️️Special Pow Attention   ⚠️️

**Do not connect AC power and the serial connection at the same time** 
The Gnd connection of the Pow has a 50% chance of being connected to the live AC wire. Connecting serial with your PC will fry your PC.

**Also do not connect any additional sensors to serial pins until you are 100% sure.**
It can at least destroy your Pow!

## Serial Connection

<img alt="picture of the Sonoff Pow PCB, the needed pin header can be seen on the top end" src="https://www.itead.cc/wiki/images/e/e3/Sonoff-Pow-00.JPG" width="40%" align="right" />

Please see the [Hardware Preparation](https://github.com/arendst/Sonoff-Tasmota/wiki/Hardware-Preparation) page for general instructions.

As always, you need to access the serial interface. The **four serial pins** (3V3, Rx, Tx, GND) are available at the rear/short end of the PCB as can be seen in the upper part of the image to the right.

## Calibration
Sonoff Pow might need calibration as correct measurements are influenced by hardware and timing differences.

I used the following procedure to calibrate.

1. Prerequisites
    - Calibrated multimeter :wink: 
    - Optional calibrated power meter :wink: 
    - 60W light bulb
2. Connect the Pow to the optional power meter.
3. Connect the 60W light bulb
4. Open a webbrowser to Pow showing the main page and another webbrowser showing the Console
5. Turn power on and wait a few seconds for the pow to settle on a stable power reading
6. Verify the power reading with the power meter or expected 60W and if needed change the power offset value with command `HLWPcal` starting from 10000. Default value is 12530
7. Verify the voltage reading with the multimeter and if needed change the voltage offset value with command `HLWUcal` starting from 1000. Default value is 1950
8. Verify the current reading with the calculated value of P (step 6) / U (step 7) and if needed change the current offset with command `HLWIcal` starting from 2500. Default value is 3500

As an alternative to steps 6-8, if you have a known load connected (e.g. a 60W bulb on a 240V supply with 250mA current) then you can use "HLWPset 60", "HLWUset 240" and "HLWIset 250" to have the Sonoff-Pow auto-calibrate to these values.

## Result explanation
The Sonoff Pow can provide Energy, Power, Voltage and Current information in different ways.

### Push result using Telemetry
The preffered way is using the periodic telemetry data. Setting ```teleperiod 300``` will send telemetry data every 5 minutes.
```
tele/pow1/ENERGY = {"Time":"2017-09-12T10:32:07", "Total":5.331, "Yesterday":0.152, "Today":0.067, "Period":0.5, "Power":6.1, "Factor":0.43, "Voltage":223.7, "Current":0.063}
```

### Pull result using status message 8
To request information you can use command ```status 8```.
```
stat/pow1/STATUS8 = {"StatusPWR":{"Total":5.331, "Yesterday":0.152, "Today":0.068, "Power":5.8, "Factor":0.26, "Voltage":219.0, "Current":0.101}}
```

### Meaning
The presented information has the following meaning:
```
Message   | Unit | Description
----------|------|-----------------------------------------------------
Total     | kWh  | Total Energy usage including Today
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
Debugging the Sonoff Pow is a bit tricky as the serial interface has a **direct connection to one of the AC power lines**. I designed below schematic using two opto couplers seperating the AC connection on the **left** from the low voltage connection on the **right** allowing for serial control at 115200 baud and uploading of firmware up to 57600 baud while AC is connected.
<img alt="OptoSerial" src="https://github.com/arendst/arendst.github.io/blob/master/media/OptoSerial.jpg" /> 

## Self Protection for Sonoff Pow

ITEAD published a recall notice for the Sonoff Pow on march 1st 2017. Some units produced in december 2016 and january 2017 are not well suited for 16A. If you have one of these units you can decide to use them anyway by limiting the maximum current in software.
It is, in fact,  possible to set a Maximum Power Threshold for the Sonoff Pow.
 If the power measured by the device exceed the threshold set by the MQTT command MaxPower for a number of seconds set by the command MaxPowerHold the device will remain switched off for MaxPowerWindow seconds (to let it cool down, for example).

For all details see [Issue #218](https://github.com/arendst/Sonoff-Tasmota/issues/218)
