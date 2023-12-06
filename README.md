# Korgi for Orca

Korgi for Orca is a tool that converts CC messages from a MIDI controller and sends them to the Orca MIDI sequencer as UDP commands. This allows you to "bind" MIDI knobs or sliders to Orca coordinates, operate on the values with Orca operators and change the values in real time with your knobs or sliders.

## Compatibility

Korgi for Orca should work with any MIDI device that sends CC messages and any MIDI interface that works with ALSA. Currently only tested on Linux. 

## Building

`cmake .`

`cmake --build .`

## Using

Create a korgi.conf. A minimal config would look something like this:

```knob 25
knob 19```

This will map 2 knobs, CC 25 and CC 19. They will both be scaled from 0-35. They will be placed in a column started at 4,1. 

Start orca, and then start korgi. When you change your knobs you should see the values appear in Orca. No midi routing is needed and the external MIDI transport and clock funtionality in Orca will still work, even when using the same device as korgi.


## Configuration

Korgi reads its configuration from a plain text file, by default `korgi.conf` in the current directory. An alternative file name can be specified as the only supported parameter on the command line.

The configuration file is expected to contain a single directive per line. Empty lines are ignored, comments start with the # symbol. In most cases, double quoted strings are allowed.

Available directives:

`connect <address> [port]`: specifies the IP address and UDP port to send the packets to. If no connect directive is supplied korgi will connect to 127.0.0.1:49160, which is the Orca default.

`device <id>`: specifies the MIDI device ID to use, starting at 0 (on Windows). Note: Windows support is currently untested

`device_name <name>`: specifies the MIDI device name (on Linux). Note: run `amidi -l` to get device names. Do not include `MIDI 1` if your device name ends with this. 

`device_map <name>`: specifies the mapping from control names to MIDI channels. Currently, only one mapping is supported, for the `nanoKONTROL2` device. Without a mapping, you can specify controls by their channel index.

`knob|slider <CC> <max> <min> <x> <y>`: maps a knob or slider to the specified X Y coords in Orca value will be scaled to min and max. There is no difference between a "knob" and a "slider" on the MIDI side, the different names are provided for convenience. If you don't supply X and Y knob values will be arranged in the top left corner in a column, 4 from the left to give room for X operators to move the values where you need them. If max and min are not supplied values 35 and 0 will be used. 
