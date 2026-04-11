# bldc-driver

ESP32 (Arduino framework) firmware that drives a three-phase BLDC motor via ESP32 MCPWM, reads a three-channel hall encoder on interrupts, runs PID speed control with a Simple Kalman filter on current/RPM feedback, and exposes WiFi OTA update and BLE services.

## Hardware

- ESP32 (`esp32dev`)
- Three-phase gate driver wired to `H1-H3` / `L1-L3` pins
- Hall encoder on `ENCODER_A/B/C`
- Door/limit inputs: `CLOSE`, `OPEN`, `STOP`, `FTS`, `FAULT`

## Features

- MCPWM-based three-phase commutation at 20 kHz
- Hall encoder rotation tracking via GPIO interrupts
- PID speed control (`br3ttb/PID`) with Kalman-filtered feedback
- Over-current detection
- OTA firmware update over HTTP (`ESP32httpUpdate`)
- BLE server (`BLEDevice`/`BLEServer`)
- EEPROM-backed persistent settings

## Layout

```
src/
  main.cpp       setup, pin config, interrupt wiring
  motor.cpp      MCPWM commutation
  encoder.cpp    hall encoder ISR and state
  rpmCounter.cpp RPM calculation
  rotation.cpp   rotation/limit handling
  current.cpp    current measurement
  serial.cpp     serial protocol
  globals.cpp
include/         matching headers and definitions.h (pin map, constants)
platformio.ini
```

## Build

PlatformIO project. From the repo root:

```bash
pio run                 # build
pio run -t upload       # flash over USB at 921600
pio device monitor      # 115200 baud
```

## Dependencies

Pulled in via `platformio.ini`:

- `suculent/ESP32httpUpdate`
- `denyssene/SimpleKalmanFilter`
- `br3ttb/PID`
