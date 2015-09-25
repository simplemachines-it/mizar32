# Introduction #

The eLua modules are extensions to Lua that are provided by the eLua interpreter.
Each one is written in C and included in the interpreter to provide a family of functions to access features of the hardware or to provide fast and convenient versions of commonly-used operations.

Two interdependent mechanisms are use when configuring the build system of eLua, both of which are in `src/platform/avr32/MIZAR32/mizar32_conf.h`

The first selects sections of code that are to be included in the interpreter or not, with names starting with `BUILD_`, listed at http://www.eluaproject.net/en_building.html

The second is the list of Lua entities to access these modules, enabled in the definition of `LUA_PLATFORM_LIBS_ROM`, listed at http://www.eluaproject.net -> "Generic modules"

# Build options #
If you compile your own firmware from the source code (see [CompilingElua](CompilingElua.md) or [the Mizar32 Web Builder](http://builder.simplemachines.it)), here are some of the options that you can select to control the features included in the interpreter that will be produced, how large they are and how well they work.

## Optional features ##
| **Name** | **Size** | **Comments** | **Status** |
|:---------|:---------|:-------------|:-----------|
| BUILD\_MMCFS | 12800    | Required for access to SD card | OK         |
| BUILD\_XMODEM | 1024     | Load programs over the serial port | OK         |
| BUILD\_SHELL | 2560     | Interactive shell to "DIR" the SD card etc | OK         |
| BUILD\_ROMFS | 1024     | Includes your programs in the eLua binary | Not required |
| BUILD\_TERM | 512      | Option required for term module | OK         |
| BUILD\_CON\_GENERIC | 512      | Necessary to control eLua via the serial port | OK         |
| BUILD\_UIP | 5960     | TCP/IP Ethernet code | OK         |
| BUILD\_CON\_TCP | 4        | If BUILD\_UIP, control eLua via telnet | Not required |
| BUILD\_RPC | 8736     | Allow control of other eLua boards via serial port | Not required |
| BUILD\_LUA\_INTERRUPTS | 1536     | Needs hooks compiled in | OK         |

## Generic modules ##
| **Name** | **Size** | **Requires** | **Comments** | **Status** |
|:---------|:---------|:-------------|:-------------|:-----------|
| adc      | ?        | BUILD\_ADC   | Read the analog-digital converters | OK         |
| bit      | 1024     | -            | Library of bitwise operations | OK         |
| can      | -        | -            | Controller Area Network | Mizar32 has no CAN hardware |
| cpu      | 1024     | -            | Read and write words to memory | OK         |
| elua     | 0        | -            | Access to emergency garbage collector interface | OK         |
| i2c      | ?        | -            |              | OK         |
| net      | 5264     | BUILD\_UIP   | TCP/IP module | OK         |
| pack     | 2048     | -            | Encode data for platform-independent transmission | OK         |
| pd       | 0        | -            | Platform data: CPU, chip and board names | OK         |
| pio      | 3136     | -            | Control GPIO pins | OK         |
| pwm      | ?        | -            |              | OK         |
| rpc      | 8736     | BUILD\_RPC   | Control an eLua server board over the serial port | Not necessary |
| spi      | 2560     | -            | SPI interface | OK but untested |
| term     | 2076     | BUILD\_TERM  | Curses-style library to move the cursor on-screen. | OK         |
| tmr      | 1024     | -            | Real-time delays | OK         |
| uart     | 1536     | -            | Low-level access to serial port | OK         |

## Memory allocators ##
eLua has a choice of three memory allocators:
  * "`simple`" - the smallest of the three, but with our 32MB of memory eLua takes five seconds to start up
  * "`newlib`" - 2632 bytes bigger than "simple", startup is immediate
  * "`multiple`" - 5608 bytes bigger than "simple", startup is immediate. Necessary when there are multiple memory regions.

We currently use "`newlib`" and only use the 32MB SDRAM for eLua data storage.
The 32KB static RAM is used for statically-sized program data and the runtime stack (which only leave 8KB free anyway).