

# Hardware design documents #

## Boards ##
  * Mizar32: The main board
  * Display\_module: The 16x2-character LCD display add-on module
  * Ethernet\_module: The ethernet and real-time clock add-on module
  * Serial\_interface: The RS232/RS485 UART add-on module
  * VGA: The Propellor-based VGA/keyboard/mouse/mic/headphone add-on module

## Documents ##
  * Kicad\_files: The original master plans for the Kicad EDA program, from which the following are generated:
  * schematic: The circuit diagram of the board as a PDF
  * gerber: The layout of the tracks on the physical board to be manufactured
  * silkscreen: The lettering to be printed on the top and bottom of the board

# Mizar32 firmware #

## mizar32-firmware-20101001.tgz ##
The first release of the Mizar32 firmware. To program it to the board you will need an AVR32 JTAG MKII; see README.txt in the archive.

  * Lua libraries: coroutine debug io package string table
  * eLua modules: pio tmr

## mizar32-firmware-20101214.tgz ##
This firmware is updated to current eLua svn head ([revision 853](https://code.google.com/p/mizar32/source/detail?r=853)) and is configured to include all currently working eLua modules except "pack"

  * Lua libraries: `coroutine debug io package string table`
  * eLua modules: `bit cpu elua pd pio spi tmr uart`

It also adds support for FAT filesystem on SD/MMC card.

## mizar32-firmware-20110320.tgz ##
An update to the 128KB versions for Mizar32 C, including as an alternative the "emBLOD" second-stage bootloader, which loads the floating-point version of eLua from a file on the SD card into RAM and executes it there.

  * Lua libraries: `coroutine io math(1) package string table`
  * eLua modules: `bit cpu elua pack pd pio spi term tmr uart`

It also adds UART buffering so you can paste programs into the console, and contains scripts to program the firmware to the board over a USB cable.

(1) The integer version of the math library has: `abs ceil floor huge min max random randomseed sqrt`.

## mizar32-firmware-20120123.tgz ##
The version for the first prdduction release including versions for the Mizar32 A and B models.

  * Mizar C integer version
    * Lua libraries: `coroutine io math(2) package string table`
    * eLua modules: `adc bit cpu elua i2c pack pd pio pwm spi term tmr uart mizar32.lcd`
  * Mizar A and B integer and floating point versions
    * Lua libraries: `coroutine debug io math package string table`
    * eLua modules: `adc bit cpu elua i2c net pack pd pio pwm spi term tmr uart mizar32.lcd`
    * Lua interrupts for UART\_RX, GPIO pins and Timers
    * eLua shell with XMODEM support
    * Console on USB CDC virtual serial port instead of the first serial port (I think! If not in this one, certainly in the next release)

(2) The integer version of the math library has: `abs ceil floor huge min max pow random randomseed sqrt`.

The emBLOD bootloader and programming via JTAG are not included in this release.

## mizar32-firmware-20130516.tgz ##
This release is based on eLua 0.9, and has the following improvements:

  * it includes the mizar32.rtc module to handle the Real Time Clock
  * `pio.decode()` for port X now returns port 23 and pin numbers 0 to 39 instead of the crazy values from earlier releases:
| PX00-PX04 | Port 3 pins 04-00 |
|:----------|:------------------|
| PX05-PX10 | port 2 pins 31-26 |
| PX11-PX14 | Port 3 pins 13-10 |
| PX15-PX34 | Port 2 pins 25-06 |
| PX35-PX39 | Port 3 pins 09-05 |