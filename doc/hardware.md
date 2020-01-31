# Pineapple II

## Overview

Pineapple II has followirng ports.

* 4x GeekPort II (Analog and Digital I/O)
* MIDI In
* MIDI Out+ (MIDI Out + XLED)
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

#### Type 2 (Hirose HR-10A)

| Pinout | Meaning       | Alternative mode |
|--------|---------------|------------------|
| 1      | Power         | Power            |
| 2      | GND           | GND              |
| 3      | Plug detector | PWM out          |
| 4      | Analog in     | Digital I/O      |
| 5      | I2C SDA       | Digital I/O      |
| 6      | I2C SCL       | Digital I/O      |

### Maintenance Port (2.5mm phone jack)

| Pinout | Meaning |
|--------|---------|
| Tip    | D-      |
| Ring 1 | D+      |
| Ring 2 | GND     |
| Sleeve | Vbus    |

## Back Panel

### MIDI OUT+ (DIN connector)

| MIDI Pin | Meaning | 1/8 Pin |
|----------|---------|---------|
| M1       | TX+     |         |
| M2       | GND     | Sleeve  |
| M3       | TX-     |         |
| M4       | Send    | Ring    |
| M5       | Return  | Tip     |

### MIDI IN+ (DIN connector)

| MIDI Pin | Meaning | 1/8 Pin |
|----------|---------|---------|
| m1       | Vcc     |         |
| m2       | GND     |         |
| m3       | Vcc     |         |
| m4       | Send    | Ring    |
| m5       | Return  | Tip     |

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

| Pin Group     | Pin      | Arduino Micro | Connect to         | Via                | Bus    |
|---------------|----------|---------------|--------------------|--------------------|--------|
| **MIDI**      | IN       | D0 (RX)       | m5                 | B11, B20           | SERL   |
|               | OUT      | D1 (TX)       | M1, M3, M5         | B12, B14, B16      | SERL   |
| **I2C**       | SDA      | D2            | Gx-5               | F05, F11. F17, F23 | ISQC   |
|               | SCL      | D3 (PWM)      | Gx-6               | F06, F12, F18, F24 | ISQC   |
| **Analog**    | A1       | A0            | G1-4/g1-R1         | F04                | ANLG   |
|               | A2       | A1            | G2-4/g2-R1         | F10                | ANLG   |
|               | A3       | A2            | G3-4/g3-R1         | F16                | ANLG   |
|               | A4       | A3            | G4-4/g4-R1         | F22                | ANLG   |
|               | PULLUP   | A4            | NC                 | NC                 | ANLG   |
| **Detector**  | D1       | D6/A7 (PMW)   | G1-3/g1-R2         | F03                | DTCT   |
|               | D2       | D9/A9 (PWM)   | G2-3/g2-R2         | F09                | DTCT   |
|               | D3       | D10/A10 (PMW) | G3-3/g3-R2         | F15                | DTCT   |
|               | D4       | D12/A11       | G4-3/g4-R2         | F21                | DTCT   |
| **Indicator** | FLED     | D11 (PWM)     | Front LED          | F25                | ---    |
| **Monitor**   | LED      | D13 (PWM)     | Back LED           | B05                | INTL   |
| **Reset**     | RST      | Reset         | SW                 | B03                | INTL   |
| **Power**     | Vin      | VIN           | P1                 | B01                | ---    |
|               | Vout     | ---           | Gx-1/gx-T          | F01, F07, F13, F19 | ---    |
|               | Vcc      | VCC           | m1, m3             | B07, B09, P01      | ---    |
|               | Vcc+R    | ---           | M4                 | B12                | ---    |
|               | GND      | GND           | Gx-2/gx-S          | F02, F08, F14, F20 | ---    |
|               |          |               | P2, M2, m2         | B02, B04, B06, B08 |        |
|               |          |               |                    | B13, P02, X07      |        |
|               | GND+R    | ---           |                    | F26                | ---    |
| **Display**   | MOSI     | MOSI          | Display 1          | X01                | DSPL   |
|               | SCLK     | SCLK          | Display 2          | X02                | DSPL   |
|               | GPIO1    | D5 (PWM)      | Display 3          | X03                | DSPL   |
|               | GPIO2    | D7            | Display 4          | X04                | DSPL   |
|               | GPIO3    | D8/A8         | Display 5          | X05                | DSPL   |
| **Internal**  | THS      | A5            | Thermal sensor     | NC                 | INTL   |
|               | RLY      | A6/D4         | Thermal breaker    | NC                 | INTL   |

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
| F25 | Indicator  | FLED A     | D11 (PWM)     |
| F26 | GND+R      | FLED K     | ---           |


### Back Connector (MIL 20p Connector)

| Pin | Meaning          | Connect to | Arduino Micro |
|-----|------------------|------------|---------------|
| B01 | DC12V            | PWR1       | Vin           |
| B02 | GND              | PWR2       | GND           |
| B03 | Reset            | SW1        | Reset         |
| B04 | GND              | SW2        | GND           |
| B05 | LED              | LEDA       | D13           |
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

### Top Connector

| Pin | Meaning |
|-----|---------|
| X01 | MOSI    |
| X02 | SCLK    |
| X03 | GPIO1   |
| X04 | GPIO2   |
| X05 | GPIO3   |

### Power Connector

| Pin | Meaning |
|-----|---------|
| P01 | Vcc     |
| P02 | GND     |
| P03 | Vdd     |
