# Toyota Hybrid CYD35 HV Health DEV1

Development branch based on the exact working `v2.4.4 RC1.4 R2.5` package supplied from the Toyota-Hybrid-CYD35-Diagnostic-CAN-Logger repo.

## Purpose

This is a display/health-development branch, not the final stripped HV Health application. It intentionally retains the R2.5 logger, diagnostic coexistence, BLE and Wi-Fi maintenance code so the new displays and S0070 decoders can be validated on the same known-working acquisition path before raw-logging functions are removed from the eventual dedicated HV Health release.

## S0070 NHW20 changes incorporated

Evidence source: 2006 Prius Gen 2 / NHW20, Autel MAXIAP200 session S0070.

Confirmed 7E2/7EA 21C3 fields now used on normal displays:
- MG2 RPM: data bytes 0-1, `BE16 - 16383` rpm
- MG2 torque: data bytes 2-3, `(BE16 - 4000) / 8` N*m
- MG1 RPM: data bytes 6-7, `BE16 - 16383` rpm
- MG1 torque: data bytes 8-9, `(BE16 - 4000) / 8` N*m
- Engine speed: data bytes 14-15, retained from existing S0034 confirmation
- MG1 motor temperature: byte 24, `byte*1.8 - 58` F
- MG1 inverter temperature: byte 25, `byte*1.8 - 58` F
- MG2 inverter temperature: byte 26, `byte*1.8 - 58` F
- MG2 motor temperature: byte 27, `byte*1.8 - 58` F

Additional S0070-confirmed read-only fields enabled:
- 21C4 converter temperature, byte 5
- 2142 auxiliary voltage, BE16/1000 V
- 211F engine runtime, BE16 seconds

## Display pages in DEV1

Page order:
1. MAIN
2. TEMP
3. HV
4. BLOCKS
5. TREND
6. POWER
7. HEALTH
8. SYSTEM
9. DIAG EXP

### MAIN
Engine coolant and MG1/MG2/ICE RPM are moved to the upper rows. SOC, HV voltage, current/power, block spread/low block and battery temperatures follow beneath.

### BLOCKS
Adds a live bar graph of all currently supported confirmed block/group voltages, with average reference, low/high identification and dV.

### TREND
Adds a 120-s RAM ring-buffer view for block-voltage spread and pack current. No SD history is required for this live plot.

### POWER
Adds large HV kW plus MG1/MG2 RPM. Gen 2 also displays the S0070-confirmed MG torques. Lower-confidence PHV MG torque/RPM remains capability-gated.

### HEALTH
Accumulates evidence time, typical/worst dV, current-polarity sample counts, current integral, thermal maximum/spread and per-block repeated-low/high behavior. Current polarity is intentionally not labeled charge/discharge until the profile sign convention is explicitly validated.

### SYSTEM
Shows coolant, intake, auxiliary voltage, converter temperature, individual MG inverter/motor temperatures, fan level, engine runtime and profile confidence.

## Threshold/alarm status

Threshold/alarm architecture is planned but no new red/yellow health verdict thresholds are activated in DEV1. This avoids presenting arbitrary limits as validated Toyota limits. Once lower/upper thresholds, persistence and hysteresis are established per metric/profile, the display-state engine and optional buzzer/speaker output can be enabled.

## Future SPEED/WHEELS pages

The SPEED and individual WHEELS pages remain future/capability-gated until a vehicle speed and/or wheel-speed source is confirmed for the target profile. Tire-size calibration and GPS reference remain in the design plan.

## Hardware validation required

This branch has received source-level structural verification but has not been compiled or vehicle-tested in this environment. First on-vehicle test should verify:
- MAIN Gen 2 MG1/MG2 RPM against Autel values.
- POWER MG1/MG2 torque direction/magnitude against Autel.
- TEMP/SYSTEM individual MG temperature labels after the S0070 byte-order correction.
- 21C4 converter temperature, 2142 auxiliary voltage and 211F runtime.
- BLOCKS min/max block number and graph behavior.
- TREND update rate and display responsiveness.
- No regression to external-tester TX inhibit, logging or Wi-Fi maintenance lifecycle.
