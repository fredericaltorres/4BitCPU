# 4 bit CPU with 4 instructions

## Components

1. Register 74LS377 or [74LS273N](http://www.ti.com/lit/ds/symlink/sn74ls273.pdf)
    - Only the first 4 bit are used
2. Adder [74S283](http://www.ti.com/lit/ds/symlink/sn74s283.pdf)
    - The chip is constanstly adding the output of register 1 and 2
3. 7 Segment Driver [74LS47](http://www.ti.com/lit/ds/symlink/sn74ls47.pdf) - (common annode)
    - Allow to display the result from register 3, only value 0..9 will be display correctly
    - Value from 10..15 will display invalid value.
A driver allowing to support number from 0..15, using hexa decimal value should be used if possible.
Displaying number 4 bit number on 2 7-segment display is possible but add more in complexity.    
4. EEPROM to store code [AT28C16](https://www.mouser.com/catalog/specsheets/atmel_doc0540.pdf)
    - A 2k EEPROM only the first 16 byte are use to store the code
    - Contain executable instruction for address 0 to 15
5. Program counter 4 bit [74LS293](http://www.ti.com/lit/ds/symlink/sn74ls293.pdf)
    Repeat indefinetly execution 0 to 15
6. Timer 55 for the clock
7. 1 PNP transistor to reverse to reverse bit 3, of the instruction set
   to activate the 7 Segment Driver.

- In a first step, I will used an Arduino Mega to simulate 4, 5, 6.
- In a second step and AT28C16 EEPROM contains 15 instructions from 0..15 will be executed
undefinetly using a 4 bit counter

## Diagram
```

                            REG 1
LOAD_REG_1 2               |-----|
            1 -----------> |CLK  |
            0              |     |
            0              |     |
            0              |     |            ADDER
            -              |     |           ------
            0 -----------> |I1 O1| --------> |    |
            1 -----------> |I2 O2| --------> |    |
            0 -----------> |I3 O3| --------> |    |
            0 -----------> |I3 04| --------> |    |
                           -------           |    |
                            REG 2            |    |           REG 3
LOAD_REG_2 3               |-----|           |    |          |-----|
            0              |     |           |    | -------> |I1   |
            1 -----------> |CLK  |           |    | -------> |I2   |
            0              |     |           |    | -------> |I3   |    7-SEG DRIVER
            0              |     |           |    | -------> |I4   |      |------|
            -              |     |           |    |          |     | ---> |      |
            0 -----------> |I1 O1| --------> |    |          |   O1| ---> |      |
            0 -----------> |I2 O2| --------> |    |          |   O2| ---> |      |
            1 -----------> |I3 O3| --------> |    |          |   O3| ---> |      |
            0 -----------> |I3 O4| --------> |    |          |   O4|  |-> |BI/RBO|
                           -------           ------          |     |  |   --------
                                      |--------------------->|CLK  |  |    |||||||
ADD_REGISTERS                         |                      -------  |    |||||||
            0                         |                            ---|   |------|
            0                         |                            |      |      |
            1 ------------------------|                            |      |------| 7 SEGMENT DISPLAY
            0                                                      |      |      |
            -                                                      |      |------|
            0                                                      |      
            0                                                      |
            0                                                      |
            0                                                      |
DISPLAY                                                            |
            0                                                      |
            0                                                      |
            0                               PNP Tran               |
            1 --------------------------------[X]------------------|
            -
            0
            0
            0
            0
NO_OPERATION
            0
            0
            0
            0
            -
            0
            0
            0
            0
```
