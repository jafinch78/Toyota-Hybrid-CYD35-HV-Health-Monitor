# DEV1 baseline

DEV1 is derived from the known-working Toyota Hybrid CYD35 Diagnostic CAN Logger v2.4.4 RC1.4 R2.5 baseline.

## Evidence incorporated

The Gen 2 Prius / NHW20 S0070 Autel MAXIAP200 IPO Decode confirms the 21C3 MG1/MG2 RPM and torque fields and resolves the individual MG motor/inverter temperature labels used by DEV1.

The R2.5 Camry hand edits remain intentional baseline content. `Code_Changes_RC2_4_4R2_5_IPO Decode Camry Data.pdf` is the provenance record for those edits.

## Development boundary

DEV1 intentionally retains the proven R2.5 acquisition, read-only diagnostic coexistence, BLE, SD logging and Wi-Fi maintenance implementation for first vehicle validation. The eventual dedicated HV Health Monitor will remove raw CAN discovery/logging functions after the health/display implementation is validated.

## Safety

No new write/control/actuator functionality is introduced. Candidate definitions do not authorize diagnostic transmission. Existing explicit read-only diagnostic and external-tester coexistence safeguards remain the baseline.

## Validation status

Source-level structural checks have been performed. Arduino/ESP32 compilation and vehicle validation are still required. DEV1 should therefore be treated as a development checkpoint, not a production vehicle release.
