# Instrument command list

### Note On

* Command ID: 01h
* Datafield: 16bit Velocity parameter + 16bit Auxilliary parameter

### Note Off

* Command ID: 02h
* Datafield: 16bit Velocity parameter + 16bit Auxilliary parameter

### Note Special A (Glissando)

* Command ID: 03h
* Datafield: 16bit Velocity parameter + 16bit New note parameter
* Can be used for non-glissango purposes if instrument or patch doesn't support glissando

### Note Special B

* Command ID: 04h
* Datafield: 16bit Velocity parameter + 16bit Auxilliary parameter

### Aftertouch

* Command ID: 05h
* Datafield: 16bit Velocity parameter + 16bit Auxilliary parameter
* Mono if note parameter set to FFFFh

### Pitch Bend

* Command ID: 06h
* Datafield: 15+1bit Fine bend, 15+1bit Coarse bend
* Fine bend is within a single semitone, coarse bend is the number of semitones
* Layout of the 15+1bit data word: The most significant bit is a sign, the rest of the bits are a 15 bit value. -0 is a valid
value.
* Mono if note parameter set to FFFFh

### Parameter Control

* Command ID: 07h
* Datafield: 16bit Parameter selector + 16bit Parameter value
* Mono if note parameter set to FFFFh
* Per-note parameter control is not mandatory for instruments to support. In that case, any per-note parameter control should
be threated as mono

# MICP system-level commands

### Acknowledge

* Command ID: 28h
* Datafield: 32bit status code
* Note is set to zero

#### Official status codes

* 0000_0000h: All Ok. Is sent on every successfully received package (unless disabled with system level command Set Protocol), 
also used to inform the sender to start file block or string transfer.
* 0000_0001h: Unsupported command or note range.
* 0000_0002h: Timeout error for file block transfer.
* 0001_0001h: Bad checksum.
* FFFF_FFFFh: Used to ping target device.

### File Block Transfer

* Command ID: 29h
* Datafield: 16 bit block size minus one (max block size = 65536) + 16bit checksum (CRC16?)
* Note value is used to indicate the remaining number of blocks
* Checksums can be disabled with system level command Set Protocol

### String Transfer

* Command ID: 2Ah
* Datafield: 16 bit string lenght minus one (max block size = 65536) + 16bit encode type
* Official encode types: ASCII = 0h, UTF8 = 1h
* Note value can indicate purpose of string. Some standard values are: 1 = load bank, 2 = load preset, 3 = save bank, 
4 = save preset

### Set Protocol

* Command ID: 2Fh
* Datafield: 32 bit flags

#### Flags

`
0: Enable/disable Acknowledge on success

1: Enable/disable Acknowledge on error

2: Enable/disable checksums on file transfer
`
