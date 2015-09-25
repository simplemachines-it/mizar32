# Introduction #

The recognition of stacked add-in modules uses the I2C bus,
an I2C multiplexer and, on the module side, an I2C EEPROM.
For this reason we have three I2C channels in Mizar32.

# Details #

The main channel is the ATMEL TWI (Two Wire Interface) bus.
On this main channel we have one device, an I2C multiplexer,
wich provides another two I2C buses, each of which has its own separate addressing.

The two additional (slave only) channels, are used to identify add-on boards and to
read the data for each module, for example the pins it uses (GPIO, serial etc.)

The module carries this information in an I2C EEPROM,
in this way, other manufacturer may use the same system
for their modules and do not have to advise us of what they are doing.

One of the beneficts of the declaration of used pins on the add-in connectors is that it may be used to avoid conflict between stacked modules.

The reason we use an I2C multiplexer is that there is a maximum number of addressable EEPROMs on an I2C bus. With the two channels we can stack and recognize up to 7 levels of board on the left and 7 on the right.

The EEPROMs that can be used are the 24LC32 and higher capacity
(the 24LC16 and lower capacity use the full EEPROM addressing space,
making only one EEPROM addressable on each I2C channel.)

Further details are on the [I2C page](I2C#I2C_address_assignment.md).