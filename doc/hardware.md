# Pineapple II

## Overview

Pineapple II has followirng ports.

* 4x GeekPort II (Analog and Digital I/O)
* MIDI In
* MIDI Out+ (MIDI Out + XLED)
* DC12V In
* Maintenance (USB)

## GeekPort II

### Type 1 (3.5mm TRRS jack)

| Pinout | Meaning       | Alternative mode |
|--------|---------------|------------------|
| Tip    | Power         | Power            |
| Ring 1 | Analog in     | Digital I/O      |
| Ring 2 | Plug detector | PWM out          |
| Sleeve | GND           | GND              |

### Type 2 (Hirose HR-10A)

| Pinout | Meaning       | Alternative mode |
|--------|---------------|------------------|
| 1      | Power         | Power            |
| 2      | GND           | GND              |
| 3      | Plug detector | PWM out          |
| 4      | Analog in     | Digital I/O      |
| 5      | I2C SDA       | Digital I/O      |
| 6      | I2C SCL       | Digital I/O      |

## Maintenance Port (2.5mm phone jack)

| Pinout | Meaning |
|--------|---------|
| Tip    | D-      |
| Ring 1 | D+      |
| Ring 2 | GND     |
| Sleeve | Vbus    |

## Front LEDs

| LED | Meaning  |
|-----|----------|
| 0/G | Status   |
| 1/W | Analog 1 |
| 2/B | Analog 2 |
| 3/Y | Analog 3 |
| 4/R | Analog 4 |

## MIDI OUT+ (DIN connector)

| MIDI Pin | Meaning | 1/8 Pin |
|----------|---------|---------|
| M1       | Vcc     |         |
| M2       | GND     | Sleeve  |
| M3       | 1-Wire  |         |
| M4       | Source  | Ring    |
| M5       | Drain   | Tip     |
| M6       | XLEDg   |         |
| M7       | XLEDb   |         |
| M8       | XLEDr   |         |

## MIDI IN+ (DIN connector)

| MIDI Pin | Meaning | 1/8 Pin |
|----------|---------|---------|
| m1       | NC      |         |
| m2       | NC      |         |
| m3       | NC      |         |
| m4       | Source  | Ring    |
| m5       | Drain   | Tip     |

## DC Jack

| DC Jack | Meaning |
|---------|---------|
| P1      | DC +12V |
| P2      | GND     |

## Reset SW/LED

| SW/LED | Meaning                          |
|--------|----------------------------------|
| LED    | Power                            |
| SW     | Reset (double-push to boot-load) |

## MPU Pinout

| Pin Group     | Pin        | Arduino Micro | Connect to         | Via                | Bus    |
|---------------|------------|---------------|--------------------|--------------------|--------|
| **MIDI**      | IN         | D0 (RX)       | m5                 | B06, B18           | SERIAL |
|               | OUT        | D1 (TX)       | M5                 | B11, B16           | SERIAL |
| **I2C**       | SDA        | D2            | Gx-5               | F05, F11. F17, F23 | I2C    |
|               | SCL        | D3 (PWM)      | Gx-6               | F06, F12, F18, F24 | I2C    |
| **Monitor**   | 0/G, XLEDg | D13 (PWM)     | LED0, XLEDg        | F25, B12           | LED    |
|               | 1/W        | D4/A6         | LED1               | F27                | LED    |
|               | 2/B, XLEDb | D5 (PWM)      | LED2, XLEDb        | F29, B13           | LED    |
|               | 3/Y        | D8/A8         | LED3               | F31                | LED    |
|               | 4/R, XLEDr | D11 (PWM)     | LED4, XLEDr        | F33, B14           | LED    |
| **Analog**    | 1          | A0            | G1-4/g1-R1         | F04                | ANALOG |
|               | 2          | A1            | G2-4/g2-R1         | F10                | ANALOG |
|               | 3          | A2            | G3-4/g3-R1         | F16                | ANALOG |
|               | 4          | A3            | G4-4/g4-R1         | F22                | ANALOG |
|               | PULLUP     | D7            | NC                 | NC                 | ANALOG |
| **Detector**  | 1          | A7/D6 (PMW)   | G1-3/g1-R2         | F03                | DETECT |
|               | 2          | A9/D9 (PWM)   | G2-3/g2-R2         | F09                | DETECT |
|               | 3          | A10/D10 (PMW) | G3-3/g3-R2         | F15                | DETECT |
|               | 4          | A11/D12       | G4-3/g4-R2         | F21                | DETECT |
| **Power**     | DC+12V     | VIN           | P1                 | B01                | ---    |
|               | Vout       | ---           | Gx-1/gx-T          | F01, F07, F13, F19 | ---    |
|               | Vcc        | VCC           | M1                 | B07                | ---    |
|               | Vcc+R      | ---           | M4                 | B10                | ---    |
|               | Vdd        | 3V            | ---                | B19                | ---    |
|               | GND        | GND           | Gx-2/gx-S          | F02, F08, F14, F20 | ---    |
|               |            |               | P2, M2             | B02, B04, B08, B20 |        |
|               | GND+R      | ---           |                    | F26, F28, F30, F32 |        |
|               |            |               |                    | F34                | ---    |
| **Reset**     | RST        | Reset         | SW1                | B03                | INTL   |
| **Internal**  | THS, 1-W   | A4            | Thermal sensor, M3 | B09                | INTL   |
|               | RLY        | A5            | Thermal breaker    | NC                 | INTL   |

## Board Connectors
### Front Connector (MIL 34p Connector)

| Pin | Meaning    | Connect to | Arduino Micro |
|-----|------------|------------|---------------|
| F01 | Vout       | G1-1/g1-T  | ---           |
| F02 | GND        | G1-2/g1-S  | GND           |
| F03 | Detector 1 | G1-3/g1-R2 | A7/D6 (PWM)   |
| F04 | Analog 1   | G1-4/g1-R1 | A0            |
| F05 | SDA        | G1-5       | D2            |
| F06 | SCL        | G1-6       | D3 (PWM)      |
| F07 | Vout       | G2-1/g2-T  | ---           |
| F08 | GND        | G2-2/g2-S  | GND           |
| F09 | Detector 2 | G2-3/g2-R2 | A9/D9 (PWM)   |
| F10 | Analog 2   | G2-4/g2-R1 | A1            |
| F11 | SDA        | G2-5       | D2            |
| F12 | SCL        | G2-6       | D3 (PWM)      |
| F13 | Vout       | G3-1/g3-T  | ---           |
| F14 | GND        | G3-2/g3-S  | GND           |
| F15 | Detector 3 | G3-3/g3-R2 | A10/D10 (PWM) |
| F16 | Analog 3   | G3-4/g3-R1 | A2            |
| F17 | SDA        | G3-5       | D2            |
| F18 | SCL        | G3-6       | D3 (PWM)      |
| F19 | Vout       | G4-1/g4-T  | ---           |
| F20 | GND        | G4-2/g4-S  | GND           |
| F21 | Detector 4 | G4-3/g4-R2 | A11/D12       |
| F22 | Analog 4   | G4-4/g4-R1 | A3            |
| F23 | SDA        | G4-5       | D2            |
| F24 | SCL        | G4-6       | D3 (PWM)      |
| F25 | LED0/G     | LED0A      | D13 (PWM)     |
| F26 | GND+R      | LED0K      | ---           |
| F27 | LED1/W     | LED1A      | D4/A6         |
| F28 | GND+R      | LED1K      | ---           |
| F29 | LED2/B     | LED2A      | D5 (PWM)      |
| F30 | GND+R      | LED2K      | ---           |
| F31 | LED3/Y     | LED3A      | D8/A8         |
| F32 | GND+R      | LED3K      | ---           |
| F33 | LED4/R     | LED4A      | D11 (PWM)     |
| F34 | GND+R      | LED4K      | ---           |

### Back Connector (MIL 20p Connector)

| Pin | Meaning          | Connect to | Arduino Micro |
|-----|------------------|------------|---------------|
| B01 | DC12V            | P1         | Vin           |
| B02 | GND              | P2         | GND           |
| B03 | Reset            | SW1        | Reset         |
| B04 | GND              | SW2        | GND           |
| B05 | MIDI IN Send     | m4         | ---           |
| B06 | MIDI IN Return   | m5         | D0 (RX)       |
| B07 | Vcc              | M1         | Vcc           |
| B08 | GND              | M2         | GND           |
| B09 | 1-W              | M3         | A4            |
| B10 | MIDI OUT Send    | M4         | ---           |
| B11 | MIDI OUT Return  | M5         | D1 (TX)       |
| B12 | XLEDg            | M6         | D13 (PWM)     |
| B13 | XLEDb            | M7         | D5 (PWM)      |
| B14 | XLEDr            | M8         | D11 (PWM)     |
| B15 | MIDI OUT2 Send   | NC         | ---           |
| B16 | MIDI OUT2 Return | NC         | D1 (TX)       |
| B17 | MIDI THRU Send   | NC         | ---           |
| B18 | MIDI THRU Return | NC         | D0 (RX)       |
| B19 | Vdd              | NC         | 3V            |
| B20 | GND              | NC         | GND           |

