# Simultype Hardware
Described below are the different categories of PCBs and 3D files used for the creation of a SimulType keyboard.  By design, each Simultype keyboard is made up of at least 2 PCBs:  
- Key Matrix: comes in the various size standards, and contains the switches, LEDS, and some supporting ICs
- Controller Board: has the main processor, and any external connectors or peripherals (USB, battery, OLEDs, encoders, etc.)
- Space Sensor Board: flex-PCB to fit inside space bar and allow capacitive touch interaction (optional)

## Dev Tools
Currently I have a basic control board for testing that simply lets you use a Pro Micro to evaluate functionality.  Due to pin count limitations, this requires using shift registers for the column lines.  This basic control board was designed with the nice! Nano in mind (or 3rd party equivalent) and supports the extra pins needed for a battery connection.  
<br>
Additionally there is a programming tool designed to use pogo pins to mate with test points on a key matrix for testing/programming of the ICs on board such as the LED driver, identification EEPROM, and capacitive touch sensor.  This board is designed to be hand-soldered and mainly used in series production, as all its communications are carried out over I2C which is available to the standard controller.  

## Creating a controller board
Key requirements for a controller to work across the widest range of Simultype key matrixes:  
- I<sup>2</sup>C (ideally high speed) needed for secondary features and LED drivers if used
- 3.3V regulated supply (low power, needed for minor peripherals)
- Higher voltage, high-current supply (needed for LEDS, may be 5V from USB or direct battery voltage of 4.2V or whatever)
- 4 GPIO for supporting low-power mode, interrupt, and addressable LEDs
- Matrix driving
  - 24 Column drivers, ideally with moderate power sourcing support for driving hall sensors on analog boards
  - 8 Row receive lines, ideally with ADC function for analog boards
  - (Optional) To drastically reduce pin counts required, column drivers can be Serial-In Parallel-Out (SIPO) shift registers, as implemented on dev controller board


## FPC Pinout
The FPC cable linking the key matrix and the controller is designed to be standard across all variants, and if anyone else wants to use it more power to you.  Here is the pinout from 1 (leftmost) to 45 (rightmost) both when viewed from above when the keyboard is assembled.  
| Pin | Name      | Function                                                                                       |
|-----|-----------|------------------------------------------------------------------------------------------------|
| 1   | COL24     | Leftmost column drive                                                                          |
| 2   | COL23     |                                                                                                |
| 3   | COL22     |                                                                                                |
| 4   | COL21     |                                                                                                |
| 5   | COL20     |                                                                                                |
| 6   | COL19     |                                                                                                |
| 7   | COL18     |                                                                                                |
| 8   | COL17     |                                                                                                |
| 9   | COL16     |                                                                                                |
| 10  | COL15     |                                                                                                |
| 11  | COL14     |                                                                                                |
| 12  | COL13     |                                                                                                |
| 13  | COL12     |                                                                                                |
| 14  | COL11     |                                                                                                |
| 15  | COL10     |                                                                                                |
| 16  | COL9      |                                                                                                |
| 17  | ROW8      | Bottommost row receive (connect to analog input on controller if possible for analog matrix)   |
| 18  | ROW7      |                                                                                                |
| 19  | ROW6      |                                                                                                |
| 20  | ROW5      |                                                                                                |
| 21  | ROW4      |                                                                                                |
| 22  | ROW3      |                                                                                                |
| 23  | ROW2      |                                                                                                |
| 24  | ROW1      | Topmost row receive (connect to analog input on controller if possible for analog matrix)      |
| 25  | GND       |                                                                                                |
| 26  | GND       |                                                                                                |
| 27  | GND       |                                                                                                |
| 28  | 3.3V      | Supply for non-LED ICs                                                                         |
| 29  | 5V        |                                                                                                |
| 30  | 5V        | Supply for LEDS (allowed to vary, must be high power)                                          |
| 31  | 5V        |                                                                                                |
| 32  | APACLK    | Clock for APA and other clock+data LEDS (may not be used on all boards)                        |
| 33  | APANEODAT | Data for Neopixel/APA LEDs (may not be used on all boards)                                     |
| 34  | SDA       | I2C data line                                                                                  |
| 35  | SCL       | I2C clock line                                                                                 |
| 36  | INT       | Shared interrupt line, active low                                                              |
| 37  | LEDOFF    | Hard shutoff for LEDs (may be used by either LED driver, or power gating for addressable LEDs) |
| 38  | COL8      |                                                                                                |
| 39  | COL7      |                                                                                                |
| 40  | COL6      |                                                                                                |
| 41  | COL5      |                                                                                                |
| 42  | COL4      |                                                                                                |
| 43  | COL3      |                                                                                                |
| 44  | COL2      |                                                                                                |
| 45  | COL1      | Rightmost column drive                                                                         |
