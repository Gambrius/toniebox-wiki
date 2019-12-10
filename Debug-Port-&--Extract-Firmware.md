# Debug Port
## General
The debug port runs on 3.3V
## Layout
`0 1 2 3 4`

`5 6 7 8 9`
## Pinout
0. ???
1. TCK 19
2. TMS 20
3. SOP2 21 (indirect SOP0 35)
4. ???

5. GND
6. RST 32
7. VCC (3.3V)
8. RX 57
9. RX 55

# Extract Firmware
## Introduction
Grab your favourite USB-UART interface, recommending those with DTR and RTS port to automate board reset + bootflag (SOP2).
## Bootloader
To be able to extract the firmware you will need to pull SOP2 high and reset the device.
## Toolset
Use [cc3200tool](https://github.com/ALLTERCO/cc3200tool) to extract the firmware.
1. Extract full firmware `cc3200tool -p /dev/ttyUSB2 --sop2 ~dtr --reset rts read_flash firmware.dmp`
2. List files in FatFS `cc3200tool -p /dev/ttyUSB2 --sop2 ~dtr --reset rts list_filesystem`
3. Extract the files you like `cc3200tool -p /dev/ttyUSB2 --sop2 ~dtr --reset rts read_file /sys/mcuimg.bin ./sys/mcuimg.bin`