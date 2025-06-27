# Lab 6
## SPI

### Table of Contents
<br>

| Step | Description | Points |
|------|-------------|--------|
| 0.1 | Set up your environment |   |
| 0.2 | Wire and organize your breadboard |   |
| 1   | Read the datasheet | 30 |
| 2   | Bit-banging SPI with 7-segment displays | 10 |
| 2   | Use SPI to communicate with 7-segment display | 15 |
| 3   | Use SPI to communicate with LCD/OLED display | 15 |
| 4   | Automate LCD/OLED display with SPI and DMA | 15 |
| 5   | Confirm your checkoffs before leaving | * |
| 99  | Automate SPI transmits with PIO | ** |
| &nbsp; | Total: | 100 |
<br>

\* - You must get your whole lab checked off before the end of your lab section to avoid a late penalty of 20%.
\*\* - This is an optional step that introduces you to a very versatile peripheral that you can use with devices that do not typically interface with well-known peripherals.  E.g. your 7-segment displays, or a WS2812 LED strip.

### Instructional Objectives

- To understand what pulse-width modulation (PWM) is and how it is used.
- To understand how to configure and use the PWM peripheral on the RP2350.
- To implement static and varying duty cycle PWM signals.
- To convert a PWM signal to an audio signal using the LM324 op-amp and a capacitor.

### Step 0.1: Set up your environment

Make sure to clone the code repository from GitHub Classroom.  Keep in mind to add, commit and push any changes you make so that your work is accessible from a lab machine. 

Open the template in VScode and with your Debug Probe connected to your Proton, click "Upload and Monitor".  Open the Serial Monitor to see the output of your program, and press the Reset button so that it prints out again.  You should now see the following:

```text
SPI Lab Test Suite
Type 'help' to learn commands.

> 
```

You can then type `help` to learn what commands you can use to test a certain subroutine.  You will use this to demo your implementation and wiring to the TAs.

If the text doesn't appear when you click "Upload and Monitor", ensure that `autotest()` is uncommented in `main.c`.

### Step 0.2: Wire and organize your breadboard

At this stage, it is important to identify the type of LCD in your kit.  As of Fall 2025, we shipped a new LCD display as a result of a **very** turbulent market for reasons that you may be already familiar with.  If you are reusing a kit from another student, it may also be that you have the older OLED display that you borrowed from a previous student in 362.  We include instructions on how to wire and use both types of displays in this lab.

Identify the type of display you have based on the picture below, and keep in mind which one you have for the rest of this lab.  **If you are not sure, ask a TA.**

![disp-type.png](images/disp-type.png)

On the LCD display, make sure that you have a bridge of solder that connects the two pads labeled SPI on the back, as shown in the same picture.  The two IIC (I2C) pads should not be connected.  If so, you can easily correct it yourself by taking a soldering iron, melting the I2C bridge solder on to a solder wick or into a solder sucker, and then soldering the two SPI bridge pads together.  

(If you want to use the display in the I2C configuration, reverse the procedure, and use the same SCL/SDA pins for I2C.)

As we go through the lab, look for the sections that are specific to your display type.

#### 0.2.1: Wire up your 7-segment displays

So far, we've been **directly** driving the 7-segment displays via the 3 decoder address pins to select the display, and the 8 segment pins to control the segments of the selected display.  However, this takes up 11 precious pins on our Proton board, which we can now reduce to 3 pins by using SPI.

The simplest way to explain how SPI works is with a shift register.  You may recall from ECE 270 that a shift register has three important ports: a clock, reset, 1-bit data input, an optional enable input, and an n-bit data output.  At the rising edge of the clock, the register shifts in the new data bit, and all the existing bits shift out to the left.

```
+---+---+---+---+---+---+---+---+
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |  <-- data input
+---+---+---+---+---+---+---+---+
  ↑          <----            ↑
 MSB          dir             LSB
```

With SPI, we would make the following connections to our shift register:

- SPI SCK (clock) to the clock input of the shift register.
- SPI TX (data) to the data input of the shift register.
    - This may be called MOSI (Master Out Slave In) in some older documentation.
- SPI CS (chip select) to the enable input of the shift register.

We can also **chain** multiple shift registers by connecting the data output of one shift register to the data input of the next shift register.  This allows us to control multiple registers with a single SPI connection!

So if we happen to have two 8-bit shift registers, we could use 11 of those bits to act as the "pins" that we're now replacing.  

Now, let's go ahead and wire up the two 74HC595 shift registers in your kit.  Disconnect the wires between your Proton board and the 7-segment displays, and the USB-C cable to your debugger (as you need to route wires underneath it) and follow this schematic to wire up your shift registers:

![alt text](images/7seg-schem.png)

Note that we haven't connected the SPI SCK, CSn and TX pins to the Proton board.  With this lab, we'll have you figure out what pins you should use for SPI, and then you can connect them yourself to the shift registers.  There are two SPI peripherals on the RP2350, so make sure you are using three pins associated with the same SPI peripheral.  You can find the pin functions sheet [here](https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf#_gpio_functions_bank_0).  Also note the function number associated with SPI for each pin.

Make sure to plug your USB-C cable back into the debugger.

Before you proceed, ensure you have identified the type of display you have.  If you do not know, ask a TA to help you identify it.

For both displays: place your display to the left of your audio jack from lab 5.  At this point, you should have a complete row of components on the bottom row of your breadboard.  Identify a set of three SPIx pins on the opposite side of your Proton board that you can use to connect to the display, but **make sure that you are using the second SPI peripheral**.  That means if you used SPI1 for the 7-segment display, you should use SPI0 for the LCD/OLED display.

#### 0.2.2: (LCD only) Wire up your SMH1602 LCD display

Follow this schematic to wire up your LCD display:

![smh1602 wiring](images/smh1602.png)

#### 0.2.3: Specify your SPI pins in `main.c`

Finally, in your `main.c` file, set the following constants to the pin numbers you used for SPI SCK, CSn and TX for both the 7-segment display and the LCD/OLED display.  You'll find them underneath where you normally put your username:

```c
const int SPI_7SEG_SCK = 0; // Replace with your SCK pin number for the 7-segment display
const int SPI_7SEG_CSn = 0; // Replace with your CSn pin number for the 7-segment display
const int SPI_7SEG_TX = 0; // Replace with your TX pin number for the 7-segment display

const int SPI_DISP_SCK = 0; // Replace with your SCK pin number for the LCD/OLED display
const int SPI_DISP_CSn = 0; // Replace with your CSn pin number for the LCD/OLED display
const int SPI_DISP_TX = 0; // Replace with your TX pin number for the LCD/OLED display
```

> [!WARNING]
> When you are choosing pins, make sure that you do not use pins that are already being used for some purpose.  So far, you are using GP2-9 for the keypad, GP36-39 for PWM, GP45 for the ADC.  Use pins on both sides of the board so that you can easily connect them to the SPI pins on the Proton board side facing each of them.

### Step 1: Read the datasheet

SPI stands for Serial Peripheral Interface, and it is by far one of the simplest and most widely used protocols for communicating with external devices.  It is a synchronous protocol, meaning that it uses a clock signal (SCK) to synchronize the transmission of data (TX to RX) between your Proton board TX and an external device's RX.  Conversely, UART, which we'll cover next lab, is an asynchronous protocol, meaning that it does not use a clock signal and instead relies on the timing of the data bits to determine when to read or write data.

Go over [Section 12.3: SPI](https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf#section_spi) of the RP2350 datasheet to understand what the pins do, as well as the SDK functions associated with SPI in the C/C++ SDK datasheet, and answer the following questions:

### Step 5: Confirm your checkoffs before leaving

> [!CAUTION]
> Make sure you got checked off here: https://engineering.purdue.edu/ece362/checkoff/
> 
> Make sure to upload your confirmation code and verify that it is accepted by Gradescope.  You will know it is accepted if you get the points from Gradescope.
> 
> Before you leave, make sure your station is clean and that you have gathered your belongings, and then call a TA to confirm that you can leave.  Confirm that you have received your checkoffs and that your confirmation code was accepted on Gradescope before logging out and leaving for the day.
