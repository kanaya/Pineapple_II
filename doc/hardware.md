# Pineapple II

## Overview

Pineapple II has the followirng ports.

* 4x GeekPort II (Analog input)
* MIDI In+ (MIDI In with Vcc power)
* MIDI Out+ (MIDI Out with RS-422 long-distance communication capability)
* DC12V In
* Maintenance (USB)

## Front Panel

### GeekPort II

#### Type 1 (3.5mm TRRS jack)

| Pinout | Meaning       | Alternative mode |
|--------|---------------|------------------|
| Tip    | Power         | Power            |
| Ring 1 | Analog in     | Digital I/O      |
| Ring 2 | Plug detector | PWM out          |
| Sleeve | GND           | GND              |

Note: If Ring 2 is connected to Sleeve (GND), Pineapple II cut plug-in power (pull-up) of Ring 1; otherwise Pineapple II provides 4.7k-Ohm pull-up to Ring 1.

| Status of Ring 2 | Pull-up of Ring 1 | Sensor Mode                                     |
|------------------|-------------------|-------------------------------------------------|
| Open             | Yes               | Inverse. Higher voltage results lower velocity. |
| Connected to GND | No                | Linear. Higher voltage results higher velocity. |


#### Type 2 (Hirose HR-10A)

| Pinout | Meaning       | Alternative mode |
|--------|---------------|------------------|
| 1      | Power         | Power            |
| 2      | GND           | GND              |
| 3      | Plug detector | PWM out          |
| 4      | Analog in     | Digital I/O      |
| 5      | I2C SDA       | Digital I/O      |
| 6      | I2C SCL       | Digital I/O      |

Note: If Pin 3 is connected to Pin 2 (GND), Pineapple II cut plug-in power (pull-up) of Pin 4; otherwise Pineapple II provides 4.7k-Ohm pull-up to Pin 4.

| Status of Pin 3  | Pull-up of Pin 4 | Sensor Mode                                     |
|------------------|------------------|-------------------------------------------------|
| Open             | Yes              | Inverse. Higher voltage results lower velocity. |
| Connected to GND | No               | Linear. Higher voltage results higher velocity. |

### Maintenance Port (2.5mm phone jack)

| Pinout | Meaning |
|--------|---------|
| Tip    | D-      |
| Ring 1 | D+      |
| Ring 2 | GND     |
| Sleeve | Vbus    |

## Back Panel

### MIDI OUT+ (DIN connector)

| MIDI Pin | Meaning | 3.5mm Audio Plug |
|----------|---------|------------------|
| M1       | TX+     |                  |
| M2       | GND     | Sleeve           |
| M3       | TX-     |                  |
| M4       | Send    | Ring             |
| M5       | Return  | Tip              |

### MIDI IN+ (DIN connector)

| MIDI Pin | Meaning | 3.5mm Audio Plug |
|----------|---------|------------------|
| m1       | Vcc     |                  |
| m2       | GND     |                  |
| m3       | Vcc     |                  |
| m4       | Send    | Ring             |
| m5       | Return  | Tip              |

### DC Jack

| DC Jack | Meaning |
|---------|---------|
| PWR1    | DC +12V |
| PWR2    | GND     |

### Reset SW/LED

| SW/LED | Meaning                          |
|--------|----------------------------------|
| LED    | Status                           |
| SW     | Reset (double-push to boot-load) |

## MPU Pinout

| Pin Group     | Pin      | Arduino Micro | Connect to         | Via                |
|---------------|----------|---------------|--------------------|--------------------|
| **MIDI**      | IN       | D0 (RX)       | m5                 | B11, B20           |
|               | OUT      | D1 (TX)       | M1, M3, M5         | B12, B14, B16      |
| **I2C**       | SDA      | D2            | Gx-5               | F05, F11. F17, F23 |
|               | SCL      | D3 (PWM)      | Gx-6               | F06, F12, F18, F24 |
| **Analog**    | ANLG1    | A0            | G1-4/g1-R1         | F04                |
|               | ANLG2    | A1            | G2-4/g2-R1         | F10                |
|               | ANLG3    | A2            | G3-4/g3-R1         | F16                |
|               | ANLG4    | A3            | G4-4/g4-R1         | F22                |
|               | PUP      | A4            | NC                 | NC                 |
| **Detector**  | DTCT1    | D6/A7 (PMW)   | G1-3/g1-R2         | F03                |
|               | DTCT2    | D9/A9 (PWM)   | G2-3/g2-R2         | F09                |
|               | DTCT3    | D10/A10 (PMW) | G3-3/g3-R2         | F15                |
|               | DTCT4    | D12/A11       | G4-3/g4-R2         | F21                |
| **Indicator** | LED0     | D13 (PWM)     | Red LED            | L1                 |
|               | LED1     | D11 (PWM)     | Green LED          | L3                 |
| **Reset**     | RST      | Reset         | SW                 | B03                |
| **Power**     | Vin      | VIN           | P1                 | B01                |
|               | Vin+R    | ---           | SWLED              | B05                |
|               | Vout     | ---           | Gx-1/gx-T          | F01, F07, F13, F19 |
|               | Vcc      | VCC           | m1, m3             | B07, B09, V2       |
|               | Vcc+R    | ---           | M4                 | B12                |
|               | GND      | GND           | Gx-2/gx-S          | F02, F08, F14, F20 |
|               |          |               | P2, M2, m2         | B02, B04, B06, B08 |
|               |          |               |                    | B13, V1, L2        |
|               | GND+R    | ---           |                    | F26                |
| **Display**   | MOSI     | MOSI          | Display 1          | X1                 |
|               | SCLK     | SCLK          | Display 2          | X2                 |
|               | DISP1    | D5 (PWM)      | Display 3          | X3                 |
|               | DISP2    | D7            | Display 4          | X4                 |
|               | DISP3    | D8/A8         | Display 5          | X5                 |
| **Internal**  | THS      | A5            | Thermal sensor     | NC                 |
|               | RLY      | A6/D4         | Thermal breaker    | NC                 |

## Board Connectors

### Front Connector (MIL 26p Connector)

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
| F25 | Vdd        | ---        | Vdd           |
| F26 | GND        | ---        | GND           |


### Back Connector (MIL 20p Connector)

| Pin | Meaning          | Connect to | Arduino Micro |
|-----|------------------|------------|---------------|
| B01 | DC12V            | PWR1       | Vin           |
| B02 | GND              | PWR2       | GND           |
| B03 | Reset            | SW1        | Reset         |
| B04 | GND              | SW2        | GND           |
| B05 | PWRLED           | LEDA       | ---           |
| B06 | GND              | LEDK       | GND           |
| B07 | Vcc              | m1         | Vcc           |
| B08 | GND              | m2         | GND           |
| B09 | Vcc              | m3         | Vcc           |
| B10 | MIDI IN Send     | m4         | ---           |
| B11 | MIDI IN Return   | m5         | D0 (RX)       |
| B12 | MIDI TX+         | M1         | D1 (TX)       |
| B13 | GND              | M2         | GND           |
| B14 | MIDI TX-         | M3         | D1 (TX)       |
| B15 | MIDI OUT Send    | M4         | ---           |
| B16 | MIDI OUT Return  | M5         | D1 (TX)       |
| B17 | MIDI OUT2 Send   | NC         | ---           |
| B18 | MIDI OUT2 Return | NC         | D1 (TX)       |
| B19 | MIDI THRU Send   | NC         | ---           |
| B20 | MIDI THRU Return | NC         | D0 (RX)       |

### Display Connector

| Pin | Meaning |
|-----|---------|
| X1  | MOSI    |
| X2  | SCLK    |
| X3  | DISP1   |
| X4  | DISP2   |
| X5  | DISP3   |

### Power Connector

| Pin | Meaning |
|-----|---------|
| V1  | GND     |
| V2  | Vcc     |

### LED Connector

| Pin | Meaning   |
|-----|-----------|
| L1  | Red LED   |
| L2  | GND       |
| L3  | Green LED |
