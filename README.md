# micp
Musical Instrument Communication Protocol

It is intended as an open-source MIDI alternative, with fixed command packet lengths.

# General layout of command structure

```
|-----8bit---|-8bit|-6bit--|10bit|-16bit-|--32bit--|
|devTypeGroup|devID|command|chNum|noteNum|datafield|
```

 * `devTypeGroup`
Device type and group. Upper 4 bit selects the type (Fh = Sequencer, 4h = Input device, 2h = Mixer, 1h = Effect Unit, 0h = Instrument). Lower 4 bit selects the group of devices if any, otherwise must be kept at zero.

 * `devID`
Device ID. A single unit can be a composed of multiple units (eg. for channel number expansion or composite devices requiring multiple kinds of operation).

 * `command`
Command number. Range 30h-3Fh is reserved for user-defined/sysEx commands, 28h-2Fh is reserved for data communication and protocol commands.

 * `chNum`
Channel number. Can be used for other purposes in user-defined/sysEx commands, and by sequencers.

 * `noteNum`
Note of which the current command applies to. Can be used for value selector if 32bit data must be sent over in the next field. Note number FFFFh is reserved for "all notes of this channel" type of commands. If the device uses the regular 12-tone system, the first note should start at 0F00h.

 * `datafield`
Usually this field is handled as two 16bit unsigned integer data. In case of note playing, the upper 16bit is velocity, the lower 16bit is position. In case of parameter control, the upper word is selecting the given parameter, the lower word is the new value. In case of pitch bending, the values are two 15+1bit integers with a single bit prefix and with -0 enabled, the upper word is corase value of whole notes, the lower word is the fine value with the maximum of a semitone.
