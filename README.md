# Water Level Control — PID (Arduino)

**Short description**  
PID-based water level control system using an Arduino (PID_v1 library) and an HC-SR04 ultrasonic sensor. The system uses two 5V DC pumps: one pump controlled by the PID loop to maintain the main tank level, and a second pump adjustable via serial commands for testing/experiments. :contentReference[oaicite:2]{index=2}

---

## Contents
- `WaterLevelControl.ino` — Arduino sketch (PID + HC-SR04 + serial control)
- `README.md` — this file
- Circuit diagram and wiring notes (in README)

---

## Authors
- Sarina Davoodi  
- Zahra Siah Chehre  
(Names taken from the original project document.) :contentReference[oaicite:3]{index=3}

---

## Components
- Arduino (Uno or equivalent)  
- HC-SR04 ultrasonic distance sensor  
- 2 × 5V DC pumps (actuated via appropriate motor drivers or MOSFETs)  
- Power supply for pumps (separate from Arduino 5V recommended)  
- Wiring, resistors, diodes as needed for pump driver protection. :contentReference[oaicite:4]{index=4}

---

## Overview
- The HC-SR04 measures the distance to the water surface.  
- The PID controller calculates the control output to drive Pump 1 (PID-controlled pump) to maintain the water level (setpoint).  
- Pump 2 speed can be set manually over the Serial Monitor for testing.  
- Serial input format: `setpoint,pumpSpeed` (e.g. `20.5,60`), where `setpoint` is desired water level in cm and `pumpSpeed` is a value in the allowed range. Invalid formats will return an error message.

---

## Wiring (basic)
- HC-SR04:
  - `Trig` -> Arduino pin 2 (OUTPUT)
  - `Echo` -> Arduino pin 3 (INPUT)
- Pump 1 control -> Arduino PWM pin 10 (via motor driver / MOSFET)
- Pump 2 control -> Arduino PWM pin 11 (via motor driver / MOSFET)
- Ground all devices together.
- **Important:** Use a motor driver or MOSFET and an appropriate power supply for the pumps — do not drive pumps directly from the Arduino 5V pin.

---

## Software / Libraries
- Arduino IDE
- `PID_v1` library (install from Library Manager)

---

## How to upload
1. Open `WaterLevelControl.ino` in Arduino IDE.
2. Install the `PID_v1` library if not present.
3. Select correct board and COM port.
4. Upload.
5. Open Serial Monitor at `115200` baud to view logs and to send `setpoint,pumpSpeed` commands.

---

## Serial command format
- Send: `setpoint,pumpSpeed\n`
  - Example: `18.0,60`  
- The code constrains pumpSpeed to a safe range (50–70 in this project) and maps it to a PWM value for Pump 2. See serial feedback messages on the monitor.

---

## PID tuning
- Default constants in the sketch: `Kp = 2.0`, `Ki = 0.5`, `Kd = 1.0`.
- Tune Kp/Ki/Kd iteratively:
  - Increase `Kp` until system responds quickly but watch for oscillation.
  - Increase `Ki` to remove steady-state error.
  - Add `Kd` to dampen oscillations.

---

## Notes & Warnings
- Pumps require a driver and separate power — do **not** power pumps directly from the Arduino.
- Check all wiring and protect circuits with diodes and fuses as appropriate.
- Adjust `tankHeight` to match your main tank (the project used 30.5 cm in the document). :contentReference[oaicite:5]{index=5}

---

## License
MIT

---
