First, you need to [install avr32gdbproxy](CompilingElua#Installing_avr32gdbproxy_on_Debian/Ubuntu.md) on your Linux box.

Then connect your Mizar32 to the computer via the AVR32 JTAGMKII ICE box, program the flash with your `.elf` or `.hex` file and go:
```
$ avr32gdbproxy &       # listens on port 4711
$ avr32-gdb *elf        # whatever .elf you programmed into the board
gdb> target extended-remote localhost:4711
gdb> run
```

That should launch eLua on the board and give you a prompt on the serial port.