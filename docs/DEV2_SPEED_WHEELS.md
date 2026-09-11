# DEV2 SPEED + WHEELS

DEV2 extends the functioning DEV1 / R2.5-derived session logger with the planned SPEED and WHEELS display architecture.

## Page order

1. MAIN
2. SPEED
3. WHEELS
4. TEMP
5. HV
6. BLOCKS
7. TREND
8. POWER
9. HEALTH
10. SYSTEM
11. DIAG EXP

## SPEED

- Large MPH display.
- Explicit source label with AUTO / VEHICLE / WHEEL AVG / future GPS architecture.
- AUTO preference: validated vehicle speed, then validated four-wheel average, then GPS.
- Compact engine temperature, ICE RPM, SOC, HV voltage, block dV, and HV power below the speed readout.
- Displays `--- MPH` when no validated speed source is fresh.

## WHEELS

- FL / FR / RL / RR speeds in MPH.
- Per-wheel calculated RPM from configured tire geometry.
- Four-wheel average speed.
- Maximum wheel-to-wheel delta.
- Initial tire geometry setting: 195/65R15.

## Evidence boundary

S0075 was the ABS/VSC/TRAC discovery session. Later project context identifies S0077 as the wheel-speed confirmation session and S0078 as independent corroboration. The exact S0077/S0078 byte-level mapping was not retrievable from the current Library/chat sources during this DEV2 build.

DEV2 therefore does not insert a guessed NHW20 wheel decoder or reuse the cross-generation Gen 3 `7B0/2103` definitions. It adds decoder-facing `setValidatedVehicleSpeedMph(...)` and `setValidatedWheelSpeedsMph(...)` contracts. The pages activate when a profile-validated decoder supplies those values.

## Preservation

The functioning DEV1 acquisition/session logger, S0070 Gen 2 MG decoding, external-tester coexistence, BLE, SD logging, Wi-Fi maintenance behavior, and safety boundaries are retained.

## Validation

Before treating SPEED/WHEELS as active NHW20 production displays, merge the exact S0077/S0078 wheel mapping and verify all four wheel positions/speeds against Autel, including low-speed turns and reverse behavior.
