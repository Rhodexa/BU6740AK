# BU6740AK
My own reverse engineering of the BU6740K/BU6740AK that HP used to employ for driving the UI board on some of their printers.
-
Other people have already done a huge part of the work:
- https://zukjeff.blogspot.com/2014/11/reverse-engineer-hp-lcd-protocol.html
- https://cocus.rf.gd/2017/05/04/hp-3550-glcd-hack/
- https://kbiva.wordpress.com/2013/04/19/hp-color-laserjet-1600-front-panel/
---
This is my take, with pinouts and headaches. My board uses a different display, so I tried to understand the IC better in orther to properly use it.

## Pinout
![screenshot](BU6740.png)

## LCD
The LCD address seems to be anything between
- $1000-$3000 (Command Write)
- $5000-7000 (Data Write)

| Pin | Type | Function | Notes |
|---|---|---|---|
| 40 | O | LCD /CS | Pulses LOW when writing to LCD |
| 41 | O | LCD Latch | Pulses HIGH when writting to LCD |
| 42 | O | LCD A0 | Command/Data select input. Pulses HIGH when using the Data address |
| 43 | O | LCD RST | Resets the LCD. Pulses when the address is between $A000-$AFFF |

# LEDs
There are two output ports you can use:
Pins 9, 10, 11, 12 are Open Drain outupts. Pressumably these can sink more current and are probably meant to drive LEDs.
These can be accessed using address $90x0 where 'x' bits correspond to a pin.
x = [Pin 9, 10, 11, 12]

# Push-Pull outputs
Pins 14, 15, 16, 17 are Push-Pull outputs. These probably can't drive much current. My particular board uses Pin 17
together with a transistor to drive a backlight for the GLCD.
These can be accessed via $900x

# Buttons
Insterestingly, the chip actually has an active, 4x8 button matrix scanner module.
- Pins 33, 34, 35 and 36 are the scanning outputs.
- Pins 18 to 25 are the inputs/returns from the keyboard matrix.

The chip stores the keys in a buffer. 

# Oscillator
The chip has an internal oscillator, which speed can be set by tweaking the resistor between pins 1 and 2.
This oscillator runs the kayboard scanner and also the timing of the LCD
