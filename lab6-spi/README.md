# Lab 6
## SPI

### Table of Contents
<br>

| Step | Description | Points |
|------|-------------|--------|
| 0.1 | Set up your environment |   |
| 0.2 | Wire and organize your breadboard |   |
| 1   | Read the datasheet | 20 |
| 2   | Bit-banging SPI with 7-segment displays | 20 |
| 3   | Use SPI to communicate with 7-segment display | 20 |
| 4   | Automate 7-segment display handling with SPI and DMA | 20 |
| 5   | Use SPI to communicate with LCD/OLED display | 20 |
| 6   | Confirm your checkoffs before leaving | * |
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

> [!NOTE]
> In this datasheet, the terms "master" and "slave" are used to refer to the devices that control and respond to the communication, respectively.  The master device initiates the communication and controls the clock signal, while the slave device responds to the master's commands.
> 
> However, these terms are increasingly being replaced with "controller/peripheral", "initiator/responder", "primary/secondary", or "host/device".  The key thing to remember is that the primary device controls the communication by setting up the clock signal, and the secondary device responds to the primary's commands.

Go over [Section 12.3: SPI](https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf#section_spi) of the RP2350 datasheet to understand what the pins do, as well as the SDK functions associated with SPI in the C/C++ SDK datasheet, and answer the following questions:

1. (5 points) What are the four signals used in SPI communication?  Which one are we not using?  Why would not use that pin for display communication?
2. (5 points) How many devices and their types can be connected to a single SPI bus?  (Hint: think about how you can use the CSn pin.)
3. (5 points) What is the maximum clock frequency for SPI on the RP2350?  
4. (5 points) Which SDK function initializes the SPI peripheral?  What are the parameters that you need to pass to it?
5. (3 points) What SDK function is used to set the parameters for SPI configuration, specifically the data format, CPOL/CPHA, endianness (bit order)?
6. (2 points) What SDK function is used to send 16-bit data through a certain SPI peripheral?

### Step 2: Bit-banging SPI with 7-segment displays

"Bit-banging" is a technique where you manually control the GPIO pins yourself to implement SPI, rather than relying on the SPI peripheral.  We teach you this technique because you can use bit-banging for *any* digital protocol, e.g. PWM.  However, it is a last-resort measure when you have run out of peripherals but still have free GPIO pins to use.  We also teach you this technique because it is a great way to understand how SPI works at a low level.

In `display.c`, do the following:

Implement `display_init_bitbang` to initialize the 3 pins for communicating with the 7-segment display as GPIO outputs.  Set the CSn pin high and the SCK and TX pins low so that we don't accidentally send any data to the shift registers.

Implement `display_bitbang_spi` to perform the actual SPI communication bit-banging.  The message to be sent are the 8 11-bit elements of `msg`.  For each of the 8 elements, you will push out 3 bits that correspond to the 3 decoder address pins, and then 8 bits that correspond to the segments of the selected display.  

How SPI works in a nutshell is as follows:
- CSn (chip select) is pulled low to select the device.
- TX (data) is set to the data to be sent, whether that's a logic 1 or 0.
- SCK (clock) is toggled high to shift the data out to the device.
- The process is repeated for each bit of data to be sent.
- After all bits of one element are sent, sleep for 1 millisecond to allow the current propagation to settle.
- After all 8 elements are sent, CSn is pulled high to deselect the device.

In `main.c`, make sure `STEP2` is defined.  This will call both functions for you in `main()`.  Upload your code and you should see the 7-segment displays light up with the numbers 0-7, each on a different display.  

> [!IMPORTANT]
> Show your TA your working 7-segment displays and that you are bit-banging SPI.  Show them the code you wrote, and that it passes the `bitbang` test case.
> 
> Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number.

### Step 3: Use SPI to communicate with 7-segment display

Now, it's time for the real thing!

In `display.c`, implement `display_init_spi` to initialize the three GPIO pins to be used by the SPI peripheral associated with them.  **This means you have to set the function for the pin accordingly, and it is no longer GPIO output.**  It must also initialize the associated SPI peripheral with the following parameters:
- The clock frequency should be set to 125 KHz.
  - Start with 125, but you can experiment and see what frequency works.  We generally recommend a lower one to let the data bits propagate through the shift registers into the 7-segment displays.
- The data format should be set to 16 bits.
  - We're only sending 11 bits at a time, but we need to send 16 at a time so that 1) the SPI peripheral can send the data in one go, and 2) the two 8-bit shift registers will receive the data correctly.
- The CPOL and CPHA should be set to 0, meaning that the clock is idle low, and data is sampled on the rising edge of the clock.
- The endianness (bit order) should be set to MSB first, meaning that the most significant bit is sent first, which is the A2 pin of the decoder on your 7-segment display.

In the same file, implement `display_print` to send each element of `msg` to the SPI peripheral.    (Now that we're using SPI, things get a lot simpler!)  Since we're using a lower clock speed, there's no need to sleep between sending each element, as the SPI peripheral will handle the timing for us.  Of course, we can still improve on that in the next step...

Uncomment STEP3, which calls both functions for you in `main`.  Upload and monitor, and you should see the same result as before, but now the 7-segment displays are being driven by the SPI peripheral instead of bit-banging.

> [!IMPORTANT]
> Show your TA your working 7-segment displays and that you are using the SPI peripheral.  Show them the code you wrote, and that it passes the `spi` test case.
> 
> Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number.

### Step 4: Automate 7-segment display handling with SPI and DMA

In the past two steps, we used a loop to continually push out the bits to the 7-segment displays.  However, we like to ensure that whatever can be offloaded, should be offloaded to the hardware separate from the CPU.  

Thus we come revisit the DMA peripheral.  You may remember from lab 4 where you used DMA to move sample data from the ADC to a variable in memory.  We'll now flip that around and use DMA to send data from memory to the SPI peripheral, which will then send it out to the 7-segment displays.

> [!IMPORTANT]
> Show your TA your working 7-segment displays and that you are using the SPI peripheral.  Show them the code you wrote, and that it passes the `dma` test case.
> 
> Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number.

### Step 5: Use SPI to communicate with LCD/OLED display

We'll now move on to the LCD/OLED display.  Make sure to look out for display-specific instructions in this step.

The display functions with an **internal controller** that interprets the commands sent to it.  We can do things like clear the display, set the cursor position, and write text at the point the cursor was set.  Therefore, before we can write text to the display, we need to initialize it and set the cursor position.  

We'll do our initialization in `chardisp.c` ("chardisp" is short for "character display").  The initialization is different for the two types of displays, so make sure to follow the instructions for your specific display type.

> [!NOTE]
> *How do you know what to do for this display?*
> There is a datasheet for your display here: https://ece362-purdue.github.io/proton-labs/datasheets/
> 
> If you read it carefully, it describes what bits you need to send to the display to initialize it, set the cursor position, and write text to it.  

#### 5.1: `init_chardisp_pins`

Initialize the pins that you have connected to your LCD/OLED display in `init_chardisp_pins` as SPI-function pins.  Initialize the associated SPI peripheral with the following parameters:
- The clock frequency should be set to 10 KHz.
  - Having a very low clock frequency will make sure that the display has enough time to process the commands sent to it.
- The data format should be set to:
  - **9 bits for the LCD** OR; 
  - **10 bits for the OLED**.
- CPOL and CPHA should be set to 0, as usual.
- The endianness (bit order) should be set to MSB first, as usual.

#### 5.2: `send_spi_cmd`

In this function, wait until the SPI peripheral (whose associated object passed in as an argument) is not busy, and write the 16-bit value `cmd` into the SPI data register.  This will send the command to the display.

#### 5.3: (LCD only) `send_spi_data`

This function just calls `send_spi_cmd` but it ORs in **0x100** to the value.  The presence of this bit indicates that it is a data byte, not a command byte, to the display's controller.

#### 5.3: (OLED only) `send_spi_data`

This function just calls `send_spi_cmd` but it ORs in **0x200** to the value.  The presence of this bit indicates that it is a data byte, not a command byte, to the display's controller.

#### 5.4: (LCD only) `cd_init`

We're now ready to initialize the LCD display.  We'll give you the list of command to send to the display to properly initialize it, but you need to go through the LCD datasheet to understand how to do this.  **Be prepared to explain to your TA what each command does and how you found it.**

1. Sleep 1 millisecond to let power to the display stabilize, in case we have only just plugged in the Proton.
2. Perform a Function Set command with the following parameters:
   - 8-bit interface
   - 2 line display mode
   - 11 dots per character (this doesn't really matter, but we'll state it to make sure it's consistent.)
3. Sleep 40 microseconds.
4. Perform a Display On/Off command where you turn on the display, turn off the cursor, and turn off the cursor blink.
5. Sleep 40 microseconds.
6. Perform a Clear Display command to clear the display.
7. Sleep 2 milliseconds.  (Clearing the display is relatively slow.)
8. Perform an Entry Mode Set command where you move the cursor to the right and increment the DDRAM address by 1 (the LCD's DDRAM is what holds your displayed characters when you send them), but do not shift the display.
9. Sleep 40 microseconds.

#### 5.4: (OLED only) `cd_init`

We're now ready to initialize the OLED display.  We'll give you the list of command to send to the display to properly initialize it, but you need to go through the OLED datasheet to understand how to do this.  **Be prepared to explain to your TA what each command does and how you found it.**

1. Sleep 1 millisecond to let power to the display stabilize, in case we have only just plugged in the Proton.
2. Perform a Function Set command with the following parameters:
   - 8-bit length
   - English/Japanese character set
3. Perform a Display On/Off command where you turn on the display, turn off the cursor, and turn off the cursor blink.
4. Perform a Clear Display command to clear the display.
5. Sleep 2 milliseconds.  (Clearing the display is relatively slow.)
6. Perform an Entry Mode Set command where you move the cursor to the right and increment the DDRAM address by 1 (the OLED's DDRAM is what holds your displayed characters when you send them), but do not shift the display.
7. Perform a Return Home command to return the cursor to the home position (beginning of the top line).

#### 5.5: `cd_display1`

This function will send the first 16 characters of the string argument `str` to the first line of your display.  Send the command that moves the cursor to the first line using `send_spi_cmd`, and then send each character in `str` to the display using `send_spi_data`.  

#### 5.6: `cd_display2`

Same as `cd_display1`, but you send the 16 characters to the second line of your display.

Carefully check your code, and then head back to `main.c` and uncomment `STEP5`, which will call all the functions you just implemented.  Upload your code and you should see your display show a reaffirming message!

> [!IMPORTANT]
> Show your TA your working display and that you are using the SPI peripheral.  Show them the code you wrote, and that it passes the `chardisp` test case.
> 
> Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number. 

### Step 6: Confirm your checkoffs before leaving

> [!CAUTION]
> Make sure you got checked off here: https://engineering.purdue.edu/ece362/checkoff/
> 
> Make sure to upload your confirmation code and verify that it is accepted by Gradescope.  You will know it is accepted if you get the points from Gradescope.
> 
> Before you leave, make sure your station is clean and that you have gathered your belongings, and then call a TA to confirm that you can leave.  Confirm that you have received your checkoffs and that your confirmation code was accepted on Gradescope before logging out and leaving for the day.

### Step 99: (Optional) Automate SPI transmits with PIO 

PIO, or Programmable Input/Output, is a special peripheral unique to the Raspberry Pi microcontrollers, including the RP2350.  It goes one step up from bit-banging in that you can write **instructions** to implement whatever protocol you want, which takes away the need to worry about exact timing constraints, which can be difficult when manipulating pins.  

When you tackle projects, PIO can be incredibly useful as you can use it to implement data transmission protocols that are not natively supported by the RP2350, such as the ones needed to communicate with WS2812 LED strips, Adafruit LED matrices, etc. or even custom protocols that you may need for your project.

![pio structure](images/pio.png)

A PIO is similar to a CPU in that:
- It has a program memory from which it fetches, decodes and executes instructions.
- It has a "scratch memory" that can be used to temporarily store data.
- It can configure and drive GPIO pins and can trigger or handle DMA requests.

A PIO is *very different* from a CPU in that:
- It has a very limited instruction set, which is designed to be simple and efficient for manipulating GPIO pins.
- It cannot communicate with the rest of the system except through FIFO buffers, which also can only be used to set pin values and scratch register values, or read them.

Read over [PIO Programs](https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf#_pio_programs) and Control Flow (immediately after that) in the RP2350 datasheet to see some examples of PIO programs, and descriptions of how they operate.  In a nutshell, we write a PIO program, for example the **squarewave** example that was shown:

```pio
.program squarewave
  set pindirs, 1 ; Set pin to output
again:
  set pins, 1 [1] ; Drive pin high and then delay for one cycle
  set pins, 0 ; Drive pin low
  jmp again ; Set PC to label `again`
```

That PIO program gets compiled to a **binary format** with a program called `pioasm`, or PIO Assembler, into a binary format.  To make including it in your project easier, we've added code to detect if you have `.pio` files in your `src` directory, and uses `pioasm` to compile them into `.pio.h` header files that will get automatically included in your project.

Then, in your `main.c`, you use the definitions created in that header file to load the compiled PIO program into the PIO instruction memory, and start the PIO state machine which fetches, decodes and executes the instructions in the PIO program.  You can configure additional things like a different PIO clock speed to make it slower, the pins to be configured by the PIO, 