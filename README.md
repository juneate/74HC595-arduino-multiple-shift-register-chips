# Arduino: Multiple Register Chips to Manage Lights

The inspiration for this project came from a group of students building a prototype for an immersive environment project that utilized a number of LEDs to light constellation patterns using a variety of physical inputs.

In working to help these student better understand how the 74HC595 shift register chip works in series, I came up with the following lesson and example. The lesson below hopefully uses language that is approachable for all levels of learners with a basic understand of mathematics and the Arduino. Please feel free to reach out with comments for improvement or questions.

## First, a quick refresher...

A `bit` is a binary number (0 or 1).⋅⋅
A `byte` is a number made of 8-bits.⋅⋅
A `decimal` number is our standard set of numbers (base-10 place holders).

### Bytes

A byte can be as low as 0 and as high as 255 in decimal (which is `11111111` in binary).

The digits in a byte translate to decimal by adding the number in the column heading for each of the bits in the byte that are turned on (set to 1), for example:

```
128 64 32 16  8  4  2  1
  1  0  1  1  0  1  1  0  ==  128 + 32 + 16 + 4 + 2  ==  182
```

Therefor, `10110110` in binary is `182` in decimal (and vice versa). Since a byte is 8-bits, storing `182` in a byte is effectively also like storing `10110110` if we look at it in binary. We just treat the byte however it's convenient for us.

### Extra

Just for fun, an `int` (integer, or whole number) is 16 bits, but if we forgo the need to store negative whole numbers, we can store twice as many bits (32) because we don't have to also keep track of the positive or negative _sign_ as part of the number. Appropriate, that's called an `unsigned int`.

## Registers

A register chip job is to take in a power source and divide the power into 8 equal streams who's `on` or `off` state is determined by that stream's correspondence to one of the 8 binary digits in the byte. By changing 8 bits of the binary pattern, we can turn certain streams (in our example, lights) on (`1`) or off (`0`).

Multiple registers can be chained together, but will require keeping track of multiple bytes, each one's 8-bits corresponding to one of the registers. Each time we want to change the pattern of any of the registers (ie, change the light pattern in any way at all), we need to re-write the pattern to all of the registers. Hence we want to keep track of all of those patterns for each register.

## Multi-register logic

So if we have a two byte `array` (multiple variables stored together as one, like a table or spreadsheet) where each byte's value is set to decimal 0, we end up with something like this (with each line of 8-bit byte stored in the array representing the pattern for one of the registers):

```
00000000
00000000
```

The pattern above tells us that all of the stream lines of power are closed, meaning all the lights are off. Our goal is to change one or more of those bits in the byte's binary pattern to a 1, which will open the stream(s) for that register and turn the lights connected to the respective pins on. So for example, if we wanted to see the light go on in the 1st register at the 4th pin from the left, the pattern looks like this:

```
00010000
00000000
```

If we were working in decimal, that would mean the bytes in the array need to be set to 16 (`00010000`) and 0 (`00000000`), respectively. Fortunately we don't have to worry about working with decimal numbers on the Arduino (which can be confusing in a hurry). We can use ​​the `bitWrite()` function, which can just pick one of the byte patterns in the array, specify which digit of that pattern needs to be changed (0-7, from left to right of the pattern) and to what binary number (0 or 1), leaving it in an 8-bit binary pattern.

Every time we want to make a change to a light, we have to write to all the registers in a row (it will write the chips in reverse order from the one furthest to the board), meaning it's not enough to just write one bit or one register alone. We have to keep track of all patterns in our array and then write all of the patterns back to the registers in a loop each time there's a change.

## The setup

The setup below was built and tested using [Tinkercad](https://www.tinkercad.com), which is a great way to build and test your projects online for free. Feel free to use and modify the file `multiple_register.brd` to see and test the project for yourself.

![Arduino setup with multiple register chips](multiple_register.png?raw=true "Arduino setup with multiple register chips")

## Credits

Below are some other great resources to check out if you want learn more. I used parts of code from all of the articles below:

- [https://www.arduino.cc/en/Tutorial/ShiftOut](https://www.arduino.cc/en/Tutorial/ShiftOut)
- [http://www.instructables.com/id/Arduino-16-LEDs-using-two-74HC595-shift-registers-/](http://www.instructables.com/id/Arduino-16-LEDs-using-two-74HC595-shift-registers-/)
- [http://adam-meyer.com/arduino/74HC595](http://adam-meyer.com/arduino/74HC595)
