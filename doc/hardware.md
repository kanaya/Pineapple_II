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
| G3  | Detector     |
| G4  | Anaolg Input |
| G5  | I2C SDA (5V) |
| G6  | I2C SCL (5V) |

The GeekPort II uses Hirose's HR-10A connector.

If Detector is open (not connected to anything electrically), Pineapple II assumes that the sensor works in **resistance mode.** In this mode a _pull-down_ resistor attached to Analog Input is activated, and thus even if Analog Input is open, it means zero input.

If Detector is connected to GND, Pineapple II assumes that the sensor works in **voltage mode.**

If Detector is connected to GND via 4.7[kOhm] resistance, Pineapple II deactivate Analog In and activate I2C connection instead (**digital mode**). Resistance other than 4.7[kOhm] is reserved for future use. E.g. 1[kOhm] resistance is resereved for 3.3V I2C connection.

Table. Sensor modes of Pineapple II

| Detector (G3)       | Mode                              |
|---------------------|-----------------------------------|
| Open                | Current mode.                     |
| GND                 | Voltage mode.                     |
| 4.7[kOhm] pull-down | Digital mode.                     |
| 1[kOhm] pull-down   | Reserved for 3.3V I2C connection. |

Some implementation of Pineapple II has four GeekPort II _Lite_ instead of the original GeekPort II. The _Lite_ version has 4 pins as follows.

Table. GeekPort _Lite_ Pinout

| Pin | Meaning   | Position |
|-----|-----------|----------|
| g1  | Vcc (5V)  | Tip      |
| g2  | Analog In | Ring 1   |
| g3  | Detector  | Ring 2   |
| g4  | GND       | Sleeve   |

The GeekPort II _Lite_ uses 1/8-inch (3.5mm) phone jack.

Pineapple II's MIDI Out port has long-distance connection capability. A specially designed twisted-pair cable (that uses pin 1 and pin 3 of MIDI Out) can carry MIDI signals more than 100m. Design of a long-distance receiver is available with the schematic of Pineapple II.

Table. MIDI Out+ Pinout

| Pin | Meaning |
|-----|---------|
| M1  | TX+     |
| M2  | GND     |
| M3  | TX-     |
| M4  | Send    |
| M5  | Return  |

Pineapple II has an alternative operating mode that _receives_ MIDI signal and drives up to 4 digital outputs. Pineapple II's MIDI In port has power supply (pin 1 and pin 2) so that it can drive a long-distance receiver without installing separate power supply.

Table. MIDI In+ Pinout

| Pin | Meaning  |
|-----|----------|
| m1  | Vcc (5V) |
| m2  | GND      |
| m3  | Aux      |
| m4  | Send     |
| m5  | Return   |

If the pin 3 of MIDI In+ is connected to GND at boot time, Pineapple II switches to _alternative_ mode.

Table. Working mode of Pineapple II

| Aux (m3) | Mode                                      |
|----------|-------------------------------------------|
| Open     | Default mode. Sensor to MIDI Out.         |
| GND      | Alternative mode. MIDI In to digital out. |

When Pineapple II operates in _alternative_ mode, GeekPort II works as follows.

Table. GeekPort II in _alternative_ mode

| Pin | Meaning        |
|-----|----------------|
| G1  | Vcc (5V)       |
| G2  | GND            |
| G3  | Digital out    |
| G4  | N/A            |
| G5  | I2C SDA (5V)   |
| G6  | I2C SCL (5V)   |

Pineapple II also has SparkFun's _Qwiic_ compatible connector on logicboard. Qwiic compatible sensors (3.3V) can be connected to the logicboard.

Table. Qwiic-compatible connector.

| Pin | Meaning      |
|-----|--------------|
| Q1  | GND          |
| Q2  | Vdd (3.3V) |
| Q3  | SDA (3.3V) |
| Q4  | SCL (3.3V) |

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

The MIDI Out+ connector is upper compatible to the regular MIDI Out connector, and has extra TX+ and TX- pins at M1 and M3 respectively.

| MIDI Pin | Meaning |
|----------|---------|
| M1       | TX+     |
| M2       | GND     |
| M3       | TX-     |
| M4       | Send    |
| M5       | Return  |

Some manufacturer uses non-standard 1/8-inch (3.5mm) MIDI jacks and allows regular audio cables to connect between MIDI systems. The audio-plug version of MIDI Out connector should be looked like as follows.

| MIDI Pin (audio plug) | Meaning |
|-----------------------|---------|
| Tip                   | Return  |
| Ring                  | Send    |
| Sleeve                | GND     |

#### MIDI In+ (DIN connector)

The MIDI In+ connector is upper compatible to the regular MIDI In connector, and has extra Vcc, GND, and Aux pins at M1, M2, and M3 respectively.

| MIDI Pin | Meaning |
|----------|---------|
| m1       | Vcc     |
| m2       | GND     |
| m3       | Aux     |
| m4       | Send    |
| m5       | Return  |

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

#### Reset SW/LED

| SW/LED | Meaning                          |
|--------|----------------------------------|
| LED    | Status                           |
| SW     | Reset (double-push to boot-load) |

### MPU Pinout

The core of Pineapple II is an _Arduino Micro._

| Pin Group     | Pin      | Arduino Micro | Connect to      | Qwiic Pro Micro |
|---------------|----------|---------------|-----------------|-----------------|
| **MIDI**      | TX       | D1 (TX)       | M1, M3, M4, M5  | TX              |
|               | GND      | GND           | M2              | GND             |
|               | RX       | D0 (RX)       | m4, m5          | RX              |
|               | Vcc      | VCC           | m1              | Vcc             |
|               | GND      | GND           | m2              | GND             |
| **I2C**       | SDA      | D2            | Gx-5            | Qwiic           |
|               | SCL      | D3 (PWM)      | Gx-6            | Qwiic           |
| **Analog**    | ANLG1    | A0            | G1-4/g1-R1      | A0              |
|               | ANLG2    | A1            | G2-4/g2-R1      | A1              |
|               | ANLG3    | A2            | G3-4/g3-R1      | A2              |
|               | ANLG4    | A3            | G4-4/g4-R1      | A3              |
|               | PDN      | A4            | ---             | (snip)          |
|               | AREF     | AREF          | ---             | (snip)          |
|               | Vout     | ---           | Gx-1/gx-T       | ---             |
|               | GND      | GND           | Gx-2/gx-S       | GND             |
| **Detector**  | DTCT1    | D6/A7 (PMW)   | G1-3/g1-R2      | D6/A7 (PWM)     |
|               | DTCT2    | D9/A9 (PWM)   | G2-3/g2-R2      | D9/A9 (PWM)     |
|               | DTCT3    | D10/A10 (PMW) | G3-3/g3-R2      | D10/A10 (PWM)   |
|               | DTCT4    | D12/A11       | G4-3/g4-R2      | D4/A6           |
| **Indicator** | LED0     | D13 (PWM)     | Green LED       | D3 (PWM)        |
|               | LED1     | D11 (PWM)     | Red LED         |                 |
| **Reset**     | RST      | Reset         | SW              | RST             |
| **Power**     | Vin      | VIN           | P1              | 5V              |
|               | GND      | GND           | P2              | GND             |
|               | Vin+R    | ---           | SWLED           | ---             |
|               | Vcc      | VCC           | m1, m3          | Vcc             |
| **Display**   | DISP1    | D5 (PWM)      | X1              | D5 (PWM)        |
|               | DISP2    | D7            | X2              | D7              |
|               | DISP3    | D8/A8         | X3              | D8/A8           |
|               | DISP4    | MOSI          | X4              | D16             |
|               | DISP5    | SCLK          | X5              | D15             |
| **Internal**  | THS      | A5            | Thermal sensor  | D2              |
|               | RLY      | D4/A6         | Thermal breaker | (snip)          |

### Board Connectors

#### Front Connector (MIL 26p Connector)

| Pin | Meaning    | Connect to |
|-----|------------|------------|
| F01 | Vout       | G1-1/g1-T  |
| F02 | GND        | G1-2/g1-S  |
| F03 | Detector 1 | G1-3/g1-R2 |
| F04 | Analog 1   | G1-4/g1-R1 |
| F05 | Vout       | G2-1/g2-T  |
| F06 | GND        | G2-2/g2-S  |
| F07 | Detector 2 | G2-3/g2-R2 |
| F08 | Analog 2   | G2-4/g2-R1 |
| F09 | Vout       | G3-1/g3-T  |
| F10 | GND        | G3-2/g3-S  |
| F11 | Detector 3 | G3-3/g3-R2 |
| F12 | Analog 3   | G3-4/g3-R1 |
| F13 | Vout       | G4-1/g4-T  |
| F14 | GND        | G4-2/g4-S  |
| F15 | Detector 4 | G4-3/g4-R2 |
| F16 | Analog 4   | G4-4/g4-R1 |


#### Back Connector (MIL 20p Connector)

| Pin | Meaning          | Connect to |
|-----|------------------|------------|
| B01 | DC9V             | PWR1       |
| B02 | GND              | PWR2       |
| B03 | Reset            | SW1        |
| B04 | GND              | SW2        |
| B05 | PWRLED           | LEDA       |
| B06 | GND              | LEDK       |
| B07 | Vcc              | m1         |
| B08 | GND              | m2         |
| B09 | Aux              | m3         |
| B10 | MIDI IN Send     | m4         |
| B11 | MIDI IN Return   | m5         |
| B12 | MIDI TX+         | M1         |
| B13 | GND              | M2         |
| B14 | MIDI TX-         | M3         |
| B15 | MIDI OUT Send    | M4         |
| B16 | MIDI OUT Return  | M5         |
| B17 | MIDI OUT2 Send   | ---        |
| B18 | MIDI OUT2 Return | ---        |
| B19 | MIDI THRU Send   | ---        |
| B20 | MIDI THRU Return | ---        |

#### Display Connector

| Pin | Meaning |
|-----|---------|
| X1  | DISP1   |
| X2  | DISP2   |
| X3  | DISP3   |
| X4  | MOSI    |
| X5  | SCLK    |

#### Power Connector

| Pin | Meaning |
|-----|---------|
| V1  | GND     |
| V2  | Vcc     |

#### LED Connector

| Pin | Meaning |
|-----|---------|
| L1  | LED0    |
| L2  | GND     |

## Experimental Feature

### Power Connector

Pineapple II has 2-pin PH connector pad at the back of the PCB.

### Qwiic-Compatible Connector

Pineapple II has SparkFun's Qwiic-compatible connector pad at the back of the PCB. This connector is connected to I2C bus via 3.3V-5V level shifting circuit.

To operate the Qwiic connector with 5V signals, do _not_ install Q1, Q2, RC1, RC2, RC3, RC4, and install zero-ohm resistance (or simply bridge) RZ1 and RZ2. Note that by this set up the pin 2 (Vcc) of Qwiic connector still provides 3.3V.
