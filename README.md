# RelayduinoMqttController

An [Arduino](http://arduino.cc) project to control an [Ocean Controls KTA-223 Relayduino](https://oceancontrols.com.au/KTA-223.html) via [MQTT](http://mqtt.org).


## Requirements

An Ocean Controls KTA-223 Relayduino with an ethernet shield installed.

In addition, an MQTT broker is required to running and accessible from the Arduino.

Example code is included in [extras](extras), for [openHAB](http://openhab.org) and Python, to generate the required MQTT messages to control the relays and monitor inputs.

### Notes on the Relayduino

When the jumper labeled AUTO is installed the board will reset each time a serial connection is made to the USB serial port. This should only be installed when reprogramming via the Arduino Environment, or the device will reset each time a serial connection is made to the unit.

## Configuration

The MQTT broker address is required to be defined within the source code. Set the address on line 8 of file [mqttConfig.h](RelayduinoMqttController/mqttConfig.h).

## Compiling code

### PlatformIO

The project is structured for use with [PlatformIO](http://platformio.org). If using PlatformIO, the external libraries used in the project will be automatically added, however the following additional libraries will beed to be added to the project lib driectory

- [MemoryFree](https://github.com/sudar/MemoryFree)
- [Relayduino](https://github.com/greenthegarden/Relayduino)

For example

```
cd lib
rm -rf MemoryFree/
git clone https://github.com/sudar/MemoryFree.git
rm -rf Relayduino
git clone https://github.com/greenthegarden/Relayduino.git
```

### Arduino IDE

When using the KTA-223 with the Arduino Environment select “Arduino Duemilenove w/ ATmega328”
from the “Tools->Board” menu, and install the “AUTO” jumper on the PCB.

In addition the following additional libraries are required

- [PubSubClient](https://github.com/knolleary/pubsubclient)
- [MemoryFree](https://github.com/sudar/MemoryFree)
- [Time](http://www.pjrc.com/teensy/td_libs_Time.html)
- [TimeAlarms](http://www.pjrc.com/teensy/td_libs_TimeAlarms.html)
- [Relayduino](https://github.com/greenthegarden/Relayduino)

# Using program

In order to make use of the program, MQTT structured messages must be generated.

To control each relay a messaged is required with the topic

```
relayduino/control/relay
```

and payload `x,y`, where

- `x` is the relay number (1 to 8)
- `y` is either `0` for off or an integer to specify the duration, in mimutes, the relay should be on for.

For example to swich relay 2 on for 10 minutes, a message in the following form is used

```
relayduino/control/relay 2,10
```

On the change of state of a relay, a status message is generated by the Arduino, with the topic

```
relayduino/status/relay
```

and payload, in the format `x,y`, where

- `x` is the relay number (1 to 8)
- `y` is either `0` or `1` to signifity whether the relay is off or on, respectively.

Data from the analog and digital KTA-223 inputs are available using the following MQTT topics

```
relayduino/input/analog
relayduino/input/opto
```

Other status messages are generated for reliability. See the file [mqttConfig.h](RelayduinoMqttController/mqttConfig.h) for details.

## Contact

Please let me know if you have any comments or suggesions.
