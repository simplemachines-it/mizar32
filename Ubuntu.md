

# Ubuntu support #

Ubuntu distribute a few packages that may be useful in your work with Mizar32:

## lua5.1 ##
After installing package `lua5.1` you can open a terminal and program in Lua:
```
$ lua5.1
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
> for i = 1,4 do
>> print(i)
>> end
1
2
3
4
> 
```
## dfu-programmer ##
`dfu-programmer` is a utility that lets you program new versions of the eLua firmware into the Mizar32 over a USB cable.

For further instructions, see the section on [Flashing\_firmware](Flashing_firmware.md)].

# Atmel support #

Atmel seem to have tried making Ubuntu packages of their software and then given up. The relics of their work seem to be:

## Tarballs of Ubuntu packages ##

An ancient version of their toolchain, the Atmel AVR 32-bit Toolchain 2.4.2 from 19, was provided as installer packages for many common GNU/Linux distributions and contains some extra tools that were left out of the later release. They no longer publicise it, but you may still be able to download:
  * [avr32\_gnu\_toolchain\_2.4.2\_ubuntu\_804.zip](http://www.atmel.com/Images/avr32_gnu_toolchain_2.4.2_ubuntu_804.zip) for Ubuntu 8.04 (Hardy Heron)
  * [avr32\_gnu\_toolchain\_2.4.2\_ubuntu\_904.zip](http://www.atmel.com/Images/avr32_gnu_toolchain_2.4.2_ubuntu_904.zip) for Ubuntu 9.04 (Jaunty Jackalope)
  * [avr32\_gnu\_toolchain\_2.4.2\_ubuntu\_910.zip](http://www.atmel.com/Images/avr32_gnu_toolchain_2.4.2_ubuntu_910.zip) for Ubuntu 9.10 (Karmic Koala) 32-bit/i386
  * [avr32\_gnu\_toolchain\_2.4.2\_ubuntu\_910\_x86\_64.zip](http://www.atmel.com/Images/avr32_gnu_toolchain_2.4.2_ubuntu_910_x86_64.zip) for Ubuntu 9.10 (Karmic Koala) 64-bit/amd64

The toolchain is not recommended, as it is buggier than their later releases. For how to install a more up-to-date toolchain,

## Debian/Ubuntu repositories ##
Their package repositories seem still to be in operation, and should also work with Debian as well as Ubuntu.

If you are running one of the blessed three version of Ubuntu, you may be able to add one of the following lines to `/etc/apt/sources.list`:
```
deb http://distribute.atmel.no/tools/avr32/release/ubuntu hardy main
deb http://distribute.atmel.no/tools/avr32/release/ubuntu jaunty main
deb http://distribute.atmel.no/tools/avr32/release/ubuntu karmic main
```
then go
```
apt-get update
```
or if you have a more recent version of Ubuntu (as you should!), try the `karmic` line.

These repositories allow you to install the ancient and buggy version 2.4.2 of their Atmel GNU toolchain by saying
```
apt-get install avr32-gnu-toolcain
```
which installs everything else in the repository:
> 

&lt;TT&gt;


> avr32-binutils
> avr32-gcc-newlib
> avr32-gdb
> libavrtools
> libavr32ocd
> libavr32sim
> libelfdwarfparser
> avr32program
> avr32gdbproxy
> avr32trace
> avrfwupgrade
> avr32headers
> avr32parts
> avr32-buildroot-essentials
> 

&lt;/TT&gt;


which in turn install dozens of other packages.

More usefully, you can use this to install more recent versions of the no-longer-distributed programs for [JTAG](JTAG.md) programming devices, `avr32program` and `avr32gdbproxy` than are included in the ZIP files of the section above.

# Making your own Ubuntu packages #
For instruction on how to make your own Ubuntu package of the latest release of the Atmel GNU Toolchain, see the page [CompilingElua](CompilingElua.md).