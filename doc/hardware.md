# Pineapple II

Pineapple II is an artist-friendly computer that can convert analog voltage to MIDI signal.

## Overview

Pineapple II can take upto 4 analog inputs (0-5V) and converts them to MIDI signals. An edge of the analog signal (e.g. 0V to 0.1V or 5V down to 4.9V) causes MIDI Note On signal with a corresponding note number (e.g. Analog 1 trigers F4, Analog 2 trigers A4, Analog 3 trigers C5...) A continuous change of the input voltage causes MIDI Control Change, and drop to 0V (or rise up to 5V) causes MIDI Note Off.

Up to 4 analog sensors can be attached to Pineapple II. The sensor can be (A) variable-resistance type (nonconductive when not active), (B) analog voltage output (0V when not active), or (C) I2C sensor.

The sensor port (named GeekPort II) of Pineapple II has 6 pins as follows.

Table. GeekPort II Pinout

| Pin | Meaning      |
|-----|--------------|
| G1  | Vcc (5V)     |
| G2  | GND          |
| G3  | Detector     |
| G4  | Anaolg input |
| G5  | I2C SDA (5V) |
| G6  | I2C SCL (5V) |

If all Detectors are open, Pineapple II assumes that all Analog inputs are in mode (A). A pull-up voltage is supplied to Analog input pin in mode (A). The front indicator (LED) blinks in green when Pinelappe II sends MIDI Note On signal.

If at least one of Detectors is connected to GND, Pinepple II assumes that all Analog inputs are in mode (B). Pull-up voltage is cut in mode (B). The front indicator (LED) blinks in orange when Pineapple II sends MIDI Note On signal.

If at least one of Decetors is connected to GND via 4.7k-Ohm resistor, Pineapple II ignores Analog inputs and works in mode (C).

The future version of Pineapple II will be able to work with 3.3V-I2C devices. To indicate the connected I2C sensor operates at 3.3V, 1k-Ohm resistor will be used to connect with Detector and GND.

Table. Sensor modes of Pineapple II

| Detector (G3)      | Mode                              |
|--------------------|-----------------------------------|
| Open               | (A) Sensor is variable-resistance |
| GND                | (B) Sensor is voltage output      |
| 4.7k-Ohm pull-down | (C) Sensor is I2C connected (5V)  |
| 1k-Ohm pull-down   | Reserved for 3.3V I2C connection  |

Pineapple II's MIDI Out port has long-distance connection capability. A specially designed twisted-pair cable (that uses pin 1 and pin 3 of MIDI Out) can carry MIDI signals more than 100m. Design of a long-distance receiver is available with the schematic of Pineapple II.

Table. MIDI Out+ Pinout

| Pin | Meaning |
|-----|---------|
| M1  | TX+     |
| M2  | GND     |
| M3  | TX-     |
| M4  | Send    |
| M5  | Return  |

Pineapple II has an _alternative_ operating mode that _receives_ MIDI signal and drives up to 4 digital outputs. Pineapple II's MIDI In port has power supply (pin 1 and pin 2) so that it can drive a long-distance receiver without installing separate power supply.

Table. MIDI In+ Pinout

| Pin | Meaning  |
|-----|----------|
| m1  | Vcc (5V) |
| m2  | GND      |
| m3  | Selector |
| m4  | Send     |
| m5  | Return   |

If the pin 3 of MIDI In is connected to the pin 2 of MIDI In at boot time, Pineapple II switches to _alternative_ mode.

| Selector (m3)      | Mode                                      |
|--------------------|-------------------------------------------|
| Open               | Default mode. Sensor to MIDI Out.         |
| GND                | Alternative mode. MIDI In to digital out. |

## Design

### Front Panel

Front panel of Pineapple II has 4x GeekPort II and Maintenance port.

#### GeekPort II

GeekPort II can be configured as Type 1 and Type 2.

##### Type 1 (Hirose HR-10A)

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

##### Type 2 (3.5mm TRRS jack)

| Pinout | Meaning       | Alternative mode |
|--------|---------------|------------------|
| Tip    | Power         | Power            |
| Ring 1 | Analog in     | Digital I/O      |
| Ring 2 | Plug detector | PWM out          |
| Sleeve | GND           | GND              |

#### Maintenance Port (2.5mm phone jack)

| Pinout | Meaning |
|--------|---------|
| Tip    | D-      |
| Ring 1 | D+      |
| Ring 2 | GND     |
| Sleeve | Vbus    |

### Back Panel

#### MIDI OUT+ (DIN connector)

| MIDI Pin | Meaning | 3.5mm Audio Plug |
|----------|---------|------------------|
| M1       | TX+     |                  |
| M2       | GND     | Sleeve           |
| M3       | TX-     |                  |
| M4       | Send    | Ring             |
| M5       | Return  | Tip              |

#### MIDI IN+ (DIN connector)

| MIDI Pin | Meaning | 3.5mm Audio Plug |
|----------|---------|------------------|
| m1       | Vcc     |                  |
| m2       | GND     |                  |
| m3       | Vcc     |                  |
| m4       | Send    | Ring             |
| m5       | Return  | Tip              |

#### DC Jack

| DC Jack | Meaning |
|---------|---------|
| PWR1    | DC +12V |
| PWR2    | GND     |

#### Reset SW/LED

| SW/LED | Meaning                          |
|--------|----------------------------------|
| LED    | Status                           |
| SW     | Reset (double-push to boot-load) |

### MPU Pinout

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
|               | PUP/SEL  | A4            | m3                 | B09                |
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

### Board Connectors

#### Front Connector (MIL 26p Connector)

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


#### Back Connector (MIL 20p Connector)

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
| B09 | Pull-up/Select   | m3         | Vcc           |
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

#### Display Connector

| Pin | Meaning |
|-----|---------|
| X1  | MOSI    |
| X2  | SCLK    |
| X3  | DISP1   |
| X4  | DISP2   |
| X5  | DISP3   |

#### Power Connector

| Pin | Meaning |
|-----|---------|
| V1  | GND     |
| V2  | Vcc     |

#### LED Connector

| Pin | Meaning   |
|-----|-----------|
| L1  | Red LED   |
| L2  | GND       |
| L3  | Green LED |
