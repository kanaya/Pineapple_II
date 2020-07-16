# Pineapple II

_Edition 2.0.3 for Pineapple II version 2.0.3-Alpha._

Pineapple II is an artist-friendly computer that can convert analog voltage to MIDI signal.

## Overview

Pineapple II can take up to 4 analog inputs (0-5V) and converts them to MIDI signals. An edge of the analog signal (e.g. 0V to 0.1V) causes MIDI Note On signal. Continuous change of the input voltage causes MIDI Control Change, and drop to 0V causes MIDI Note Off.

Up to 4 analog sensors can be attached to Pineapple II. Thanks to built-in pull-down resistor, the sensor can be **(1) variable voltage** (0V when not active) or **(2) variable resistance** (nonconductive when not active).

The sensor port (named GeekPort II) of Pineapple II has 6 pins as follows.

Table. GeekPort II Pinout

| Pin | Meaning      |
|-----|--------------|
| G1  | Vcc (5V)     |
| G2  | GND          |
| G3  | Anaolg Input |
| G4  | Detector     |
| G5  | I2C SDA (5V) |
| G6  | I2C SCL (5V) |

The GeekPort II uses Hirose's HR-10A connector.

If Detector is open (not connected to anything electrically), Pineapple II assumes that the sensor works in **resistance mode.** In this mode a _pull-down_ resistor attached to Analog Input is activated, and thus even if Analog Input is open, it means zero input.

If Detector is connected to GND, Pineapple II assumes that the sensor works in **voltage mode.**

Some implementation of Pineapple II has four GeekPort II _Lite_ instead of the original GeekPort II. The _Lite_ version has 4 pins as follows.

Table. GeekPort _Lite_ Pinout

| Pin | Meaning   | Position |
|-----|-----------|----------|
| g1  | Vcc (5V)  | Tip      |
| g2  | Analog In | Ring 1   |
| g3  | Detector  | Ring 2   |
| g4  | GND       | Sleeve   |

The GeekPort II _Lite_ uses 1/8-inch (3.5mm) phone jack.

## Design

### Front Panel

Front panel of Pineapple II has 4x GeekPort II and Maintenance port.

#### GeekPort II

GeekPort II can be configured as Regular or Lite.

##### Regular (Hirose HR-10A)

| HR-10A Pin Num. | Meaning   |
|-----------------|-----------|
| 1               | Power     |
| 2               | GND       |
| 3               | Detector  |
| 4               | Analog In |
| 5               | I2C SDA   |
| 6               | I2C SCL   |

##### Lite (3.5mm TRRS jack)

Some imprementation of Pineapple II would come with the _Lite_ version of the ports instead of the regular ones. The _Lite_ version doesn't support I2C connection, however, it could be a reasonable option for mass production.

| Position | Meaning   |
|----------|-----------|
| Tip      | Power     |
| Ring 1   | Analog In |
| Ring 2   | Detector  |
| Sleeve   | GND       |

Note that the _Lite_ version can be TRS (tip-ring-sleave) plug if the sensor requires **voltage** mode of Pineapple II.

#### Maintenance Port (2.5mm phone jack)

The maintenance port is electronically compatible with USB 2.0.

| Position | Meaning |
|----------|---------|
| Tip      | D-      |
| Ring 1   | D+      |
| Ring 2   | GND     |
| Sleeve   | Vbus    |

### Back Panel

#### MIDI Out+ (DIN connector)

The MIDI Out+ connector is upper compatible to the regular MIDI Out connector, and has extra TX+ and TX- pins at M6 and M8 respectively.

| MIDI Pin | Meaning |
|----------|---------|
| M1       | NC      |
| M2       | GND     |
| M3       | NC      |
| M4       | Send    |
| M5       | Return  |
| M6       | TX+     |
| M7       | NC      |
| M8       | TX-     |

Some manufacturer uses non-standard 1/8-inch (3.5mm) MIDI jacks and allows regular audio cables to connect between MIDI systems. The audio-plug version of MIDI Out connector should be looked like as follows.

| MIDI Pin (audio plug) | Meaning |
|-----------------------|---------|
| Tip                   | Return  |
| Ring                  | Send    |
| Sleeve                | GND     |

#### MIDI In+ (DIN connector)

The MIDI In+ connector is upper compatible to the regular MIDI In connector, and has extra RX+, Detect, RX- pins at m6, m7, m8 respectively. The m2 pin is connected to GND. If m7 pin is connected to m2 pin, Pineapple II switches MIDI In from m4/m5 pair to m6/m8 pair.

| MIDI Pin | Meaning |
|----------|---------|
| m1       | NC      |
| m2       | GND     |
| m3       | NC      |
| m4       | Send    |
| m5       | Return  |
| m6       | RX+     |
| m7       | Detect  |
| m8       | RX-     |


The audio-plug version of MIDI In should be looked like as follows.

| MIDI Pin (audio plug) | Meaning |
|-----------------------|---------|
| Tip                   | Return  |
| Ring                  | Send    |

#### DC Jack (EIAJ Class-4 jack)

The DC jack can be either EIAJ-4 power jack or standard 5.5mm/2.1mm power jack.

| DC Jack | Meaning |
|---------|---------|
| PWR1    | DC +12V |
| PWR2    | GND     |

#### Reset SW

| SW | Meaning                          |
|----|----------------------------------|
| SW | Reset (double-push to boot-load) |

### MPU Pinout

The core of Pineapple II is an _Arduino Micro._

| Pin Group     | Pin      | Arduino Micro | Connect to      |
|---------------|----------|---------------|-----------------|
| **MIDI**      | TX       | D1 (TX)       | M4, M5          |
|               | RX       | D0 (RX)       | m4, m5          |
|               | GND      | GND           | M2, m2          |
| **MIDI DX**   | TX       | D1 (TX)       | M6, M8          |
|               | RX       | D0 (RX)       | m6, m8          |
|               | GND      | GND           | GND             |
| **I2C**       | SDA      | D2            | Gx-5, RTC, X    |
|               | SCL      | D3 (PWM)      | Gx-6, RTC, X    |
| **Analog**    | ANLG1    | A0            | G1-4/g1-R1      |
|               | ANLG2    | A1            | G2-4/g2-R1      |
|               | ANLG3    | A2            | G3-4/g3-R1      |
|               | ANLG4    | A3            | G4-4/g4-R1      |
|               | PDN      | A4            | ---             |
|               | AREF     | AREF          | ---             |
|               | Vout     | ---           | Gx-1/gx-T       |
|               | GND      | GND           | Gx-2/gx-S       |
| **Detector**  | DTCT1    | D6/A7 (PMW)   | G1-3/g1-R2      |
|               | DTCT2    | D9/A9 (PWM)   | G2-3/g2-R2      |
|               | DTCT3    | D10/A10 (PMW) | G3-3/g3-R2      |
|               | DTCT4    | D12/A11       | G4-3/g4-R2      |
| **Indicator** | LED0     | D13 (PWM)     | Green LED       |
|               | LED1     | D11 (PWM)     | Red LED         |
| **Reset**     | RST      | Reset         | SW              |
| **Power**     | Vin      | VIN           | P1              |
|               | GND      | GND           | P2              |
| **X**         | Vcc      | Vcc           | X1              |
|               | D7       | D7            | X2              |
|               | SCL      | D3            | X3              |
|               | SDA      | D2            | X4              |
|               | GND      | GND           | X5              |
| **Y**         | DISP1    | D5 (PWM)      | Y1              |
|               | DISP2    | D8/A8         | Y2              |
|               | GND      | GND           | Y3              |
| **Internal**  | THS      | A5            | Thermal sensor  |
|               | RLY      | D4/A6         | Thermal breaker |

### Board Connectors

#### Front Connector (MIL 26p Connector)

| Pin | Meaning    | Connect to | Connect to (Lite) |
|-----|------------|------------|-------------------|
| F01 | Vout       | G1-1       | g1-T              |
| F02 | GND        | G1-2       | g1-S              |
| F03 | Detector 1 | G1-3       | g1-R2             |
| F04 | Analog 1   | G1-4       | g1-R1             |
| F05 | Vout       | G2-1       | g2-T              |
| F06 | GND        | G2-2/g2-S  | g2-S              |
| F07 | Detector 2 | G2-3/g2-R2 | g2-R2             |
| F08 | Analog 2   | G2-4/g2-R1 | g2-R1             |
| F09 | Vout       | G3-1/g3-T  | g3-T              |
| F10 | GND        | G3-2/g3-S  | g3-S              |
| F11 | Detector 3 | G3-3/g3-R2 | g3-R2             |
| F12 | Analog 3   | G3-4/g3-R1 | g3-R1             |
| F13 | Vout       | G4-1/g4-T  | g4-T              |
| F14 | GND        | G4-2/g4-S  | g4-S              |
| F15 | Detector 4 | G4-3/g4-R2 | g4-R2             |
| F16 | Analog 4   | G4-4/g4-R1 | g4-R1             |


#### Back Connector (MIL 20p Connector)

| Pin | Meaning          | Connect to |
|-----|------------------|------------|
| B01 | DC9V             | PWR1       |
| B02 | GND              | PWR2       |
| B03 | Reset            | SW1        |
| B04 | GND              | SW2        |
| B05 | NC               | NC         |
| B06 | GND              | m2         |
| B07 | MIDI IN Send     | m4         |
| B08 | MIDI IN Return   | m5         |
| B09 | MIDI RX+         | m6         |
| B10 | MIDI RX Detect   | m7         |
| B11 | MIDI RX-         | m8         |
| B12 | GND              | M2         |
| B13 | MIDI OUT Send    | M4         |
| B14 | MIDI OUT Return  | M5         |
| B15 | MIDI TX+         | M6         |
| B16 | MIDI TX-         | M8         |
| B17 | MIDI OUT2 Send   | (m4)       |
| B18 | MIDI OUT2 Return | (m5)       |
| B19 | MIDI THRU Send   | (M4)       |
| B20 | MIDI THRU Return | (M5)       |

#### I2C/Display Connector

| Pin | Meaning |
|-----|---------|
| X1  | GND     |
| X2  | SDA     |
| X3  | SCL     |
| X4  | D7      |
| X5  | Vcc     |

| Pin | Meaning |
|-----|---------|
| Y1  | GND     |
| Y2  | DISP2   |
| Y3  | DISP1   |

#### Power Connector

| Pin | Meaning |
|-----|---------|
| V1  | GND     |
| V2  | Vcc     |

#### LED Connector

| Pin | Meaning |
|-----|---------|
| L1  | LED0    |
| L2  | LED1    |
| L3  | GND     |

## Experimental Feature

### Power Connector

Pineapple II has 2-pin PH connector pad at the back of the PCB.

### Qwiic-Compatible Connector

Pineapple II has SparkFun's Qwiic-compatible connector pad at the back of the PCB. This connector is connected to I2C bus via 3.3V-5V level shifting circuit.

To operate the Qwiic connector with 5V signals, do _not_ install Q1, Q2, RC1, RC2, RC3, RC4, and install zero-ohm resistance (or simply bridge) RZ1 and RZ2. Note that by this set up the pin 2 (Vcc) of Qwiic connector still provides 3.3V.

| Kind             | Symbol | Specification      |
|------------------|--------|--------------------|
| Capacitor        | C1     | 22u                |
|                  | C2     | 470u               |
|                  | C3     | 1u                 |
|                  | C4     | 0.1u               |
|                  | C5     | 0.1u               |
|                  | C6     | 0.01u              |
|                  | C7     | 0.01u              |
|                  | C8     | 0.01u              |
|                  | C9     | 0.01u              |
|                  | C10    | 0.1u               |
|                  | C11    | 0.01u (2012)       |
|                  | C12    | 0.01u (2012)       |
|                  | C13    | 0.01u (2012)       |
| Diode            | D1     | 1S3                |
|                  | D2     | 1S3                |
|                  | D3     | 1S3                |
| Fuse             | F1     |                    |
| IC               | IC1    | 74LS07N            |
|                  | IC2    | 4066N              |
|                  | IC11   | 74LS153D           |
|                  | IC12   | LTC485             |
|                  | IC13   | LTC485             |
| Connector        | J1     | JST PH 5p          |
|                  | J2     | JST PH 3p Vertical |
|                  | J3     | JST PH 3p          |
|                  | J4     | JST PH 4p          |
|                  | J5     | JST PH 2p          |
|                  | J11    | JST SH 4p          |
|                  | J12    | JST SH 4p          |
|                  | J13    | JST SH 4p          |
|                  | J14    | JST SH 4p          |
| Jumper           | JP1    | Pinheader 1x2      |
|                  | JP2    | Pinheader 1x2      |
|                  | JP3    | Pinheader 2x3      |
|                  | JP4    | Pinheader 1x2      |
|                  | JP5    | Pinheader 1x5      |
| Relay            | K1     | G5V-1              |
| Micro Controller | MC1    | Arduino Micro      |
| Optocoupler      | OK1    | TLP552             |
| Resistor         | R1     | 47k                |
|                  | R2     | 1k                 |
|                  | R3     | 1k                 |
|                  | R4     | 120                |
|                  | R5     | 120                |
|                  | R11    | 10k (2012)         |
|                  | R12    | 120 (2012)         |
|                  | R13    | 120 (2012)         |
| Resistor Net     | RN1    | 4.7k 4-elements    |
|                  | RN2    | 10k 4-elements     |
|                  | RN3    | 220 8-elements DIL |
| Connector ML     | SV1    | 16p                |
|                  | SV2    | 20p                |
| Transistor       | T1     | 2SC1815Y           |
| Thermal Sensor   | U1     | DS18B20            |
| V. Regulator     | V1     | L7805              |
