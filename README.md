# MedusaDG
MedusaDG - Code for CBUSmedusa based on Duncan Greenwood CBUS Library.
A placeholder for further development. 

Currently CBUSnINmOUT covers most, if not all, of the rquired functionality

<img align="right" src="arduino_cbus_logo.png"  width="150" height="75">

Key Features:
- Based on CANmInnOut (Duncan Greenwwod et al)
- MERG CBUS interface.

## Overview

The program is written in C++ but you do not need to understand this to use the program.
The program includes a library that manages the LED functionality.
The program does not allow for the inclusion of a CBUS setup switch or LEDs. Consequently,
the following library versions are required:
- CBUS Version 1.1.14 or later
- CBUSConfig 1.1.10 or later
Notwithstanding the fact that they are not used, CBUSSwitch and CBUSLED libraries must be 
available for access by the CBUS and CBUSConfig libraries for reason of backwards library
compatibility.

## License

This work is licensed under the: Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit: http://creativecommons.org/licenses/by-nc-sa/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.

License summary: You are free to: Share, copy and redistribute the material in any medium or format Adapt, remix, transform, and build upon the material

The licensor cannot revoke these freedoms as long as you follow the license terms.

Attribution : You must give appropriate credit, provide a link to the license,
              and indicate if changes were made. You may do so in any reasonable manner,
              but not in any way that suggests the licensor endorses you or your use.

NonCommercial : You may not use the material for commercial purposes. **(see note below)

ShareAlike : If you remix, transform, or build upon the material, you must distribute
             your contributions under the same license as the original.

No additional restrictions : You may not apply legal terms or technological measures that
                             legally restrict others from doing anything the license permits.
** For commercial use, please contact the original copyright holder(s) to agree licensing terms

This software is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE


## Hardware

Medusa is based in the use of an Arduino Uno, a MERG kit 119, and a Grove shield

Kit 110 requires five Arduino pins to be allocated. Three of these are fixed
in the architecture of the Arduino processor, the other two are for chip select and interrupt, and are set by the hardware
to pins 10 and 2 respectively.

**It is the users responsibility that the total current that the Arduino is asked to supply 
stays within the capacity of the on board regulator.  Failure to do this will result in 
terminal damage to your Arduino.**

Pins defined as inputs are active low.  That is to say that they are pulled up by an 
internal resistor. The input switch should connect the pin to 0 Volts.

Pins defined as outputs are active high.  They will source current to (say) an LED. It is 
important that a suitable current limiting resistor is fitted between the pin and the LED 
anode.  The LED cathode should be connected to ground.

## Serial ports

This file assumes that Serial supports the debug facility.


### Library Dependencies

The following third party libraries are required:

Library | Purpose
---------------|-----------------
Streaming.h  |*C++ stream style output, v5, (http://arduiniana.org/libraries/streaming/)*
Bounce2.h    |*Debounce of switch inputs*
ACAN2515.h   |*library to support the MCP2515/25625 CAN controller IC*
CBUS2515.h   |*CAN controller and CBUS class*
CBUSconfig.h |*module configuration*
CBUS.h       |*CBUS Class*
cbusdefs.h   |*Definition of CBUS codes*
CBUSParams.h   |*Manage CBUS parameters*
CBUSSwitch.h   |*library compatibility*
CBUSLED.h      |*library compatibility*
TBA.h            |*Sound player library*

### Application Configuration

The module can be configured to the users specific configuration in a section of code 
starting at line 92 with the title DEFINE MODULE. The following parameters can be changed 
as necessary:
```
#define DEBUG 0       // set to 0 for no serial debug
```
This define at circa line 73 allows various output reports to be made to the serial monitor 
for use in debugging.  To enable these, change the value of DEBUG from 0 to 1.

```

const unsigned long CAN_OSC_FREQ = 8000000;     // Oscillator frequency on the CAN2515 board
```
If the oscillator frequency on your CAN2515 board is not 8MHz, change the number to match. The 
module will not work if this number does not match the oscillator frequency.

```
//Module pins available for use are Pins 3 - 9 and A0 - A5
const byte LED[] = {8, 7};        // LED pin connections through 1K8 resistor
const byte SWITCH[] = {9, 6};     // Module Switch takes input to 0V.
```
Insert the pin numbers being used for inputs and outputs between the appropriate pair of braces.
Pin numbers must be seperated by a comma.

### CBUS Op Codes

The following Op codes are supported:

OP_CODE | HEX | Function
----------|---------|---------
 OPC_ACON | 0x90 | On event
 OPC_ACOF | 0x91 | Off event
 OPC_ASON | 0x98 | Short event on
 OPC_ASOF | 0x99 | Short event off

### Event Variables

Event Variables control the action to take when an event is received.
The number of Event Variables (EV) is equal to the number of LEDs plus 1.

The first Event Variable (EV1) controls Start-of-Day reporting. Set EV1 to 1 to enable 
reporting of state of the switches.

The remaining Event Variables control the LEDs. 
Event Variable 2 (EV2) controls the first LED pin in the ```LED``` array. 
EV3 controls the second LED pin, etc.

The following EV values are defined to control the LEDs:

 EV Value | Function
--------|-----------
 0 | LED off
 1 | LED on
 2 | LED flash at 500mS
 3 | LED flash at 250mS
 
### Node Variables

Node Variables .

The number of Node Variables (NV)
NV1 corresponds to the volume setting 


 
## Set Up and the Serial Monitor

Without a CBUS switch, it is necessary to have another means of registering the module on 
the CBUS and acquiring a node number.  This is accomplished by sending a single character to 
the Arduino using the serial send facility in the serial monitor of the Arduino IDE (or similar).

#### 'r'
This character will cause the module to renegotiate its CBUS status by requesting a node number.
The FCU will respond as it would for any other unrecognised module.

#### 'z'
This character needs to be sent twice within 2 seconds so that its action is confirmed.
This will reset the module and clear the EEPROM.  It should thus be used with care.

Other information is available using the serial monitor using other commands:

#### 'n'
This character will return the node configuration.

#### 'e'
This character will return the learned event table in the EEPROM.

#### 'v'
This character will return the node variables.

#### 'c'
This character will return the CAN bus status.

#### 'h'
This character will return the event hash table.

#### 'y'
This character will reset the CAN bus and CBUS message processing.

#### '\*'
This character will reboot the module.

#### 'm'
This character will return the amount of free memory. 

 
 
 
 
 
