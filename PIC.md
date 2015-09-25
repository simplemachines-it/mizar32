

# Hardware view #

The LCD display board has a PIC16F84A microcontroller which is a low-power processor used to talk to the Ampire 162B LCD panel, detect button presses and communicate with the main AVR32 processor over the I2C bus.

# Software view #

The PIC16F84 runs a program that implements I2C and responds to the 8-bit command bytes `0x7C` to `0x7F` (7-bit addresses 62 and 63). The lower address accepts commands, such as "clear screen" and the higher address accepts codes for characters to be displayed on the LCD. Reading a byte from the lower address gives the current position of the cursor in the character memory, while reading a byte from the higher address gives the current state of the push-buttons.

# Further reading #
  * The [PIC16F84A datasheet](http://ww1.microchip.com/downloads/en/devicedoc/35007b.pdf)
  * [picprog](http://hyvatti.iki.fi/~jaakko/pic/picprog.html): hardware and software to program the device
  * [The source code for the Mizar32 Display Module's firmware](http://github.com/martinwguy/Mizar32_Display_Module)
  * [The LCD module wiki page](LCD.md)