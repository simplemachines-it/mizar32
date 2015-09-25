This page has moved to [wikibooks](http://en.wikibooks.org/wiki/Mizar32/Flashing_firmware).

This is the old version that was here:

---



# Flashing firmware images #

There are several ways to write new firmware images to the Mizar32:

## With the USB DFU bootloader ##

The first 8KB of the Mizar32 flash memory comes pre-programmed with Atmel's USB DFU boot loader, which is able to write to the rest of the flash memory. To talk to it, you can use Atmel's closed-source 'flip' and 'batchisp3' tools, which are awful, or the open-source 'dfu-programmer' which is OK.

### 1. Using batchisp3 ###

Atmel publishes closed-source programs to talk to the USB DFU boot loader: the graphical "flip" and the command-line "batchisp3". They are both of poor quality and only the second one is currently usable with AT32UC3 parts.

Note that the Debian/Ubuntu package "flip" is something completely different.

Installation on Ubuntu (as root) is (adapted from [eLua's AVR32 platform info](http://www.eluaproject.net/en_installing_avr32.html)):
```
apt-get install openjdk-6-jre
cd /usr/local
wget http://www.atmel.com/dyn/resources/prod_documents/flip_linux_3-2-1.tgz
tar xfz flip_linux_3-2-1.tgz
rm flip_linux_3-2-1.tgz

cat > bin/batchisp3 << \EOF
#! /bin/sh
FLIP_HOME=/usr/local/flip.3.2.1/bin
JAVA_HOME=/usr/lib/jvm/java-6-openjdk/jre
USB_DEVFS_PATH=/dev/bus/usb
export FLIP_HOME JAVA_HOME USB_DEVFS_PATH
exec /usr/local/flip.3.2.1/bin/batchisp3.sh "$@"
EOF

chmod 755 bin/batchisp3
```
(on Red Hat systems, the `USB_DEVFS_PATH` runes should be omitted).

To get `flip` to work at all, you have to `cd /usr/local/flip.3.2.1/bin` first, and it doesn't yet support AT32UC3 parts so we can only use the command-line `batchisp3` program:

  * Connect the Mizar32 to your PC with a USB cable
  * Power the Mizar32 on (or press its reset button) while holding the user button (SW2) depressed.
  * On the PC, issue the command
```
batchisp3 -hardware usb -device at32uc3a0128 -operation erase f memory flash blankcheck loadbuffer $PWD/elua_lualong_at32uc3a0128.elf program verify start reset 0
```
Note that you have to explicitly give the full pathname of the firmware file (the `$PWD/` trick here). Otherwise it looks for the firmware file in `/usr/local/flip.3.2.1/bin/`.

See also:
  * The [Atmel USB DFU Bootloader Datasheet](http://www.atmel.com/dyn/resources/prod_documents/doc7618.pdf)
  * The [FLIP home page](http://www.atmel.com/tools/FLIP.aspx)

#### Using Batchisp under Windows Vista/7 32bit ####

  * Download the latest version of [Flip](http://www.atmel.com/tools/FLIP.aspx) for Windows from Atmel's web site (`BATCHISP.EXE` is inside the Flip installer) and follow the instructions for installation. At the moment, only Batchisp supports AT32UC3A microprocessors while Flip does not support its yet, so you must install Flip but can't use it.

  * Activate the DFU Bootloader: connect the Mizar32 to your PC with a USB cable, connect the power plug and press and hold the reset button while holding the user button (SW2), then release the reset button, then release the user button.

  * Go to the Windows Control Panel, right-click on Computer → Properties → Device Manager → right-click on 'AT32UC3A DFU' → Update Driver Software → 'Browse my computer for driver software' and select the path Flip\usb (here, it's `c:\Program Files (x86)\Atmel\Flip 3.4.3\usb\`) and click OK. Now Windows tells you 'Windows can't verify the publisher of this driver software' click on 'Install this software anyway'. Now your Mizar32 driver for Batchisp is installed.

  * Open your Windows Command Processor: Start → type 'cmd' in 'Search command and file' → Right-click on cmd.exe → Start as Administrator. Type 'PATH' followed by entire path of your Batchisp.exe directory; on our machine the command is:
```
PATH c:\Program File (x86)\Atmel\Flip 3.4.3\bin 
```
  * Restart Windows. Now Windows is able to find the `Batchisp.exe` program

  * Download the latest version of [Mizar32 firmware](http://code.google.com/p/mizar32/downloads/list). Decompress this files in some directory. Run your Windows Command Processor: Start → type 'cmd' in 'Search command and file' → Right-click on cmd.exe → Start as Administrator. Type the following command (this command is case sensitive):
```
batchisp -device at32uc3aXXXX -hardware usb -operation erase f memory flash blankcheck loadbuffer \Mizar32_firmware_directory\elua_lualong_at32uc3aXXXX.elf program verify start reset 0
```
where:

  * `at32uc3aXXXX` is your Atmel device that can be: `at32uc3a0128`, `at32uc3a0256` or `at32uc3a0512`.
  * `\Mizar32_firmware_directory\` is the entire PATH where you stored the Mizar32 firmware.
  * `elua_lualong_at32uc3aXXXX.elf` is the firmware version.

For example, you can flash a Mizar32 B (the 256Kb version) with this command line:
```
batchisp -device at32uc3a0256 -hardware usb -operation erase f memory flash blankcheck loadbuffer C:\Users\Simplemachines\Desktop\project\elua_firmware\0256\elua_lualong_at32uc3a0256.elf program verify start reset 0
```

If batchisp can't run because "MSVCR71.dll is missing"
  * download the file [msvcr71.dll](https://docs.google.com/viewer?a=v&pid=explorer&chrome=true&srcid=0B3KeO9iN5LY-OGMxOGY3MWEtYjFlNC00NWE0LTlmYzAtNmUzYzI3M2RhMjFl&hl=it) into your `Flip\bin` directory (on this PC it's `c:\Program Files (x86)\Atmel\Flip 3.4.3\bin\`)
  * Re-type the batchisp command above to update Mizar32 firmware.

See also:
  * The [Atmel USB DFU Bootloader Datasheet](http://www.atmel.com/dyn/resources/prod_documents/doc7618.pdf)
  * The [FLIP home page](http://www.atmel.com/tools/FLIP.aspx)

Please report any feedback or suggestions on this procedure to support@simplemachines.it

#### Using Batchisp under Windows Vista/7 64bit ####

  * Download the latest version of [Flip](http://www.atmel.com/tools/FLIP.aspx) for Windows from the Atmel web site (Batchisp.exe is inside Flip installation). Follow the video instructions for installation. At the moment only Batchisp supports AT32UC3A microprocessors; Flip does not support it yet, so you have to install Flip but can't use it.

  * Download and unzip this [USB driver](https://docs.google.com/viewer?a=v&pid=explorer&chrome=true&srcid=0B3KeO9iN5LY-NjQ2ZmU3MTEtYWFhOS00MjYyLWJkZTUtMzUyNmZjNGU2NDhl&hl=it), because Atmel's original drivers are unsigned and Windows Vista/7 64bit does not let you install unsigned driver. Unpack the .zip file downloaded in the `Flip\usb` folder; on our machines the folder is:
```
c:\Program Files (x86)\Atmel\Flip 3.4.3\usb 
```

  * Activate the DFU Bootloader: connect the Mizar32 to your PC with a USB cable Connect the power plug and press and hold the reset button while holding the user button (SW2), release the reset button, then release the user button.

  * Go to the Windows Control Panel with: rigth-click on Computer --> Properties --> Device Manager --> right-click on 'AT32UC3A DFU' --> Update Driver Software --> 'Browse my computer for the driver software' and select the path where you copied the new usb signed driver (on this machine `c:\Program Files (x86)\Atmel\Flip 3.4.3\usb\atmel-flip-3.4.2-signed-driver`) and click OK. Now your Mizar32 driver for Batchisp is installed.

  * Open your Windows Command Processor: Start --> type 'cmd' in 'Search commands and files' --> Right-click on cmd.exe --> Start as Administrator. Type 'Path' followed by entire path of your `Batchisp.exe` directory; on our machine the command is:
```
Path c:\Program File (x86)\Atmel\Flip 3.4.3\bin
```
  * Restart Windows. Now Windows is able to find the `Batchisp.exe` program.

  * Download the lastest version of [Mizar32 firmware](http://code.google.com/p/mizar32/downloads/list). Unpack this file and run your Windows Command Processor: Start --> type 'cmd' in 'Search commands and files' --> Right-click on cmd.exe --> Start as Administrator and type the following command (this command is case sensitive):
```
batchisp -device at32uc3aXXXX -hardware usb -operation erase f memory flash blankcheck loadbuffer \Mizar32_firmware_directory\elua_lualong_at32uc3aXXXX.elf program verify start reset 0
```
where:

  * `at32uc3aXXXX` is your Atmel device that can be: at32uc3a0128, at32uc3a0256, at32uc3a0512.
  * `\Mizar32_firmware_directory\` is the entire PATH where you stored the Mizar32 firmware.
  * `elua_lualong_at32uc3aXXXX.elf` is the firmware file version.

We flash our Mizar32 B (256KB version) with this command line:
```
batchisp -device at32uc3a0256 -hardware usb -operation erase f memory flash blankcheck loadbuffer C:Users\Simplemachines\Desktop\project\elua_firmware\0256\elua_lualong_at32uc3a0256.elf program verify start reset 0
```

See also:
  * The [Atmel USB DFU Bootloader Datasheet](http://www.atmel.com/dyn/resources/prod_documents/doc7618.pdf)
  * The [FLIP home page](http://www.atmel.com/tools/FLIP.aspx)

### 2. Using dfu-programmer ###

`dfu-programmer` is an open-source program to talk to the USB DFU boot loader. It is included in Debian and Ubuntu, for which the installation step is (as root):
```
apt-get install dfu-programmer
```
Fetch the firmware:
```
wget http://mizar32.googlecode.com/files/mizar32-firmware-20110320.tgz
tar xfz mizar32-firmware-20110320.tgz
cd mizar32-firmware-20110320
```
Now
  * Connect the Mizar32 to your PC with a USB cable
  * Power the Mizar32 on (or press its reset button) while holding the user button (SW2) depressed
  * On the PC, issue the commands:
```
dfu-programmer at32uc3a0128 erase
dfu-programmer at32uc3a0128 flash elua_lualong_at32uc3a0128.hex
dfu-programmer at32uc3a0128 start
```
If it says `dfu-programmer: no device present.`, try running it as root. If so, and you want anyone to be able to run it, you can go, as root:
```
chown root $(which dfu-programmer)
chmod 4755 $(which dfu-programmer)
```
though this opens a security hole so, if you may have malicious users logged into your system, it might be better to add yourself to group "`admin`" in `/etc/group` and go:
```
chown root:admin $(which dfu-programmer)
chmod 4750 $(which dfu-programmer)
```

Note that the Debian/Ubuntu program "dfutool", included in the "bluez" package, is something completely different.

See also:
  * the [Atmel USB DFU Programmer project](http://sourceforge.net/projects/dfu-programmer) at sourceforge

#### Bug in dfu-programmer 0.5.1 ####

There is a bug in dfu-programmer v0.5.1 which very occasionally misprograms the flash. The symptom is
```
$ dfu-programmer at32uc3a0256 flash elua_lualong_at32uc3a0256.hex
Validating...
Image did not validate.
```
The bug is fixed in version 0.5.2 and later but Ubuntu and Debian currently carry v0.5.1.

You can check the version of the installed `dfu-programmer` by saying
```
dfu-programmer --version 2>&1 | head -1
```
and you can install a more recent version by compiling from source:
```
apt-get install libusb-dev build-essential
wget http://downloads.sourceforge.net/project/dfu-programmer/dfu-programmer/0.5.4/dfu-programmer-0.5.4.tar.gz
tar xfz dfu-programmer-0.5.4.tar.gz
cd dfu-programmer-0.5.4
./configure
make
sudo make install
sudo apt-get purge dfu-programmer     # remove old version
```

## With an AVR32 JTAG ICE MKII flash programmer ##

A JTAG programmer will allow you to reprogram all of the flash memory, including overwriting the USB DFU bootloader if you wish.

You will need to connect an AVR32 JTAG ICE MKII programmer to your PC with its USB interface and to the Mizar32 via the 10-pin JTAG interface. Apply power to both.

Install the AVR32 GNU toolchain and utilities (see CompilingElua) then fetch and program the firmware:
```
wget http://mizar32.googlecode.com/files/mizar32-firmware-20110320.tgz
tar xfz mizar32-firmware-20110320.tgz
cd mizar32-firmware-20110320
sh program-mizar32-jtag.sh
```
This will reprogram the entire contents of the flash memory: the USB DFU boot loader, the ISP configuration word, the general-purpose fuse bits and the eLua interpreter. It will then start the board running.