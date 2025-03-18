# DCAN-II-4020-PWM-Module
To review further information about this product please go to the [wiki](https://github.com/Dracma-Solutions/DCAN-II-4020-PWM-Module/wiki). 
### DCommands

DCommands are low level command that can be send thru serial port, follow the Serial Port Configuration section to configurate the serial port.

Use **4020_config.json** file or **4020_config(String).json** if you are willing to work with strings. Both files are in dcommands folder and contain basic frame structure for all operations on the module.

To construct a DCAN frame take the "request" property of the required operation for instance "setDutyCycle" so **"05 47 04 1B"** are the first bytes of the frame.

Then take the parameters index of this operation, in this case "dutyCycleParametersIndex" where first parameter is the channel that will be configure, in this example channel 1 and must be read as hexadecimal value, so:

- "channel" byte must be 01h (1d) 

Then take rest of the parameters; "integerPart" that represents the integer part of the duty cycle value and "fractionalPart" the 2 fractional one. Duty cycle must be read as hexadecimal on bits/s units, in this example channel 1 will be configure to **50%** that leads to **3200h**  so:

- "integerPart" byte must be 32h
- "fractionalPart" byte must be 00h

Then set those values into frame following the position described on each property, in this case "integerPart" 4 and "fractionalPart" 5 where all positions are 0 based. So frame end up being the following:

- "05 47 04 1B 01 32 00" all must be read as hexadecimal values so serial frame will be **547041B013200h**

Once this message was sent, a feedback frame must be returned, even if this frame is write only, the positive feedback is described under every operation and is constructed like request frame, in this example the positive feedback should be:

- "05 47 04 5B 01 32 00" all must be read as hexadecimal values so serial frame will be **547045B013200h**

This confirm that device receive frame correctly, note that service byte (position 3, 0 based) was return plus 40h and baudrate remains on same positions with the value that was set.

Error will return: 05 47 0x 7F...
