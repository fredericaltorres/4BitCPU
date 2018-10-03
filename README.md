# 4 bit CPU with 4 instructions

## Components

1. Register 74LS377 or 74LS273N
2. Adder 74S283
3. 7 Segment Driver [74LS47](http://www.ti.com/lit/ds/symlink/sn74ls47.pdf) - (common annode)
4. EEPROM to store code AT28C16
    - A 2k EEPROM only the first 16 byte are use to store the code
    - Contain executable instruction for address 0 to 15
5. Program counter 4 bit 74LS293 
    Repeat indefinetly execution 0 to 15
6. Timer 55 for the clock

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
            0              |     |           |    | -------> |I4   |      |-----|
            -              |     |           |    |          |     | ---> |     |
            0 -----------> |I1 O1| --------> |    |          |   O1| ---> |     |
            0 -----------> |I2 O2| --------> |    |          |   O2| ---> |     |
            1 -----------> |I3 O3| --------> |    |          |   O3| ---> |     |
            0 -----------> |I3 O4| --------> |    |          |   O4|  |-> |     |
                           -------           ------          |     |  |   -------
                                      |--------------------->|CLK  |  |   |||||||
ADD_REGISTERS                         |                      -------  |   |-----|
            0                         |                            ---|   |     |
            0                         |                            |      |     |
            1 ------------------------|                            |      |-----| 7 SEGMENT DISPLAY
            0                                                      |      |     |
            -                                                      |      |     |
            0                                                      |      |-----|
            0                                                      |
            0                                                      |
            0                                                      |
DISPLAY                                                            |
            0                                                      |
            0                                                      |
            0                                                      |
            1 -----------------------------------------------------|
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
