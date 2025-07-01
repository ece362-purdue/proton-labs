# Lab 7
## UART

### Table of Contents
<br>

| Step | Description | Points |
|------|-------------|--------|
| 0.1 | Set up your environment |   |
| 0.2 | Wire and organize your breadboard |   |
| 1   | Read the datasheet | 20 |
| 2   | Configure a UART to transmit and receive text | 10  |
| 3   | Add support for `printf()` and `fgets()` | 20  |
| 4   | Automate UART device driving | 20  |
| 5   | Design a Peripheral Configuration Shell | 30  |
| 6   | Confirm your checkoffs before leaving | * |
| 99  | Automate SPI transmits with PIO | ** |
| &nbsp; | Total: | 100 |
<br>

\* - You must get your whole lab checked off before the end of your lab section to avoid a late penalty of 20%.

### Instructional Objectives

- To understand how to configure and use the UART peripheral on the RP2350.
- To learn how to interpret UART parameters like baud rate, data bits, stop bits, and parity.
- To design and implement a simple bidirectional UART communication system.

### Step 0.1: Set up your environment

For this lab, there is no autotest.  At this point, you will be expected to debug your code by reading the datasheet, identifying important registers, using the SDK functions to figure out what you need to do, and if something goes wrong, debugging and checking register values for the UART and/or associated peripherals.

By the end of the lab, you will have learned how to create your own command shell, in the same style as the one we provided you in previous labs with autotest.

No other wiring is needed other than ensuring your Debug Probe is still connected to your Proton board, via the debug and UART pins (which should have remained in place throughout the semester).

### Step 1: Read the background + datasheet

Asynchronous serial communication using a Universal Asynchronous Receiver/Transmitter (UART) has been a staple of inter-computer communication since its introduction in the 1960s. Although asynchronous serial has been largely supplanted by more specialized communication protocols that are faster or more specially structured, it remains the only interface on your microcontroller that can be readily used for ad hoc interactive bidirectional communication. In this lab, you will use a UART to implement communication with an interactive program. You will examine the asynchronous serial protocol using an oscilloscope to identify the communication payload.

Decades ago, most mainframe computing equipment was large and was usually centralized in special rooms. Devices called serial terminals, like the one pictured below, were used to interact with the computers from a distance. The only function of these terminals was to take characters typed on a keyboard and send them through an asynchronous serial connection to the computer and receive characters back through the serial connection and display them on the screen. They were often called dumb terminals because they did not constitute a programmable computer system by themselves. All programmable computation happened on the remote computer.

![Serial Terminal](images/DEC_VT100_terminal.png)

It was very easy for a personal computer to do everything a dumb terminal could, and more. All that was needed was a program to connect the keyboard and text screen to the bidirectional serial port. This program was called a **terminal emulator**. There are several kinds available for any computer.

In lab 0, we had you connect your Debug Probe's UART pins (RX and TX) to those on your Proton board (GP0 and GP1).  With your Proton board, we've made UART very easy to use by including our custom Debug Probe as part of the board.  Your Debug Probe's firmware (from Raspberry Pi) implements a UART-to-USB bridge, presenting itself as a UART device to your computer.  

![alt text](images/devmgmt.png)

*This is Windows's Device Manager. Ports/visuals may differ based on your OS.*

We'll continue to use the same connections, but with this lab, we'll recreate the functionality of a terminal emulator on the Proton board, allowing you to send and receive text from your computer, and to implement a simple command shell that you can use to interact with your board.

When we type a key, say "A", into the Serial Monitor in PlatformIO, that key is sent as an ASCII-encoded byte (8 bits) across the wire.  It travels through the USB cable into the Debug Probe, which converts it into a UART data packet, and pushes it out through the TX pin of the Debug Probe, which reaches GP1 (UART0 RX) on the Proton board.  That transmission will look something like this:

```
Idle Bit | Start Bit | 8 Data Bits (MSB first)        | Stop Bit
    1    |     0     | 0 1 0 0 0 0 0 1 (0x41 = 'A')   |   1
```

The UART on your Proton will detect this transmission when the start bit arrives, shift the bits into its internal shift register, and when the stop bit arrives, it will make the data available to your program via a "data register", not unlike the one used by SPI.

Conversely, when you send a character from your program running on the Proton board, it will be transmitted through the TX pin (GP0) of the Proton board, to the RX pin on your Debug Probe.  The Debug Probe program will convert that into a USB packet and send it to your computer, where you will see it in the Serial Monitor.

UART typically has two other pins - RTS and CTS - which are used for hardware flow control.  These date back to a time when the UART was used to communicate with devices that were much slower than the computer, such as a printer.  The RTS (Request To Send) pin is used by the computer to indicate that it is ready to send data, and the CTS (Clear To Send) pin is used by the device to indicate that it is ready to receive data.  For the most part, you don't need these pins - you certainly didn't need them when we connected the Debug Probe to the Proton board.

Go over [12.1 UART](https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf#section_uart) as well as the relevant [SDK functions](https://datasheets.raspberrypi.com/pico/raspberry-pi-pico-c-sdk.pdf#group_hardware_uart) for UART, and answer the following questions:

1. In the code example under "hardware_uart" in the SDK, the function for GPIO is set using the macro "UART_FUNCSEL_NUM(uart0, N))" and not, as one would expect, "GPIO_FUNC_UART".  What does this macro do?  

2. With the macro above, what would be the GPIO function selected for GP0 and GP1 (and by extension, any TX/RX pins) be?  What would be the GPIO function selected for GP2 and GP3 (and by extension, any RTS/CTS pins) be?  

3. Which register holds parameters like word length, number of stop bits, parity, etc.?

4. Identify what the frequency of the clock to the baud rate generator is, and calculate the value of the Baud Rate Divisor register for a baud rate of 115200 bps.  
- Hints: try out the SDK function that sets the baud rate in your `main.c` file.  The input clock to the baud rate generator is `clk_peri`.

5. Identify the interrupt numbers associated with when the UART receives a character, and when it transmits a character.

> [!IMPORTANT]
> Show your answers for the questions asked above to your TA.  You must have **correct** answers to earn points for this step.  
> 
> Avoid the urge to ask others (AI/LLMs are included in "others") for answers.  

### Step 2: Configure a UART to transmit and receive text

In the function `init_uart`, using the SDK functions or by writing to the registers directly, configure the UART 0 peripheral to communicate at 115200 bps, with 8 data bits, no parity, and 1 stop bit.

In `main`, we've provided code that calls `init_uart`, waits for a character to be received, and then transmits that character back.  

Upload and monitor, and try typing characters into the terminal.  Instead of seeing the character you typed, you see "You typed: " followed by the character you typed.  Weird, right?

That's because, by default, **UART does not automatically echo characters back to the terminal**.  You have to write code to do that.  The code in `main` is a good start, but generally terminal emulators have more robust echoing capabilities, such as handling backspace and other control characters, which we'll do next.

> [!IMPORTANT]
> Show your TA that typing characters into the Serial Monitor does not echo them back directly, but instead shows "You typed: " followed by the character you typed.  Show them the code you wrote.  (Remember that there is no autotest.)
> 
> Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number.

### Step 5: Confirm your checkoffs before leaving

> [!CAUTION]
> Make sure you got checked off here: https://engineering.purdue.edu/ece362/checkoff/
> 
> Make sure to upload your confirmation code and verify that it is accepted by Gradescope.  You will know it is accepted if you get the points from Gradescope.
> 
> Before you leave, make sure your station is clean and that you have gathered your belongings, and then call a TA to confirm that you can leave.  Confirm that you have received your checkoffs and that your confirmation code was accepted on Gradescope before logging out and leaving for the day.