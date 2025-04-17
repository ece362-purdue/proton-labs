# Lab 0
## Setting up the Microcontroller

### Table of Contents
<br>

| Step | Description | Points |
|------|-------------|--------|
| 0 | Initialize your git repository for your lab at home | ** |
| 1 | Solder pin headers to your development board | ** |
| 2 | Place the Development Board on a Breadboard | ** |
| 3 | Install VScode and PlatformIO | ** |
| 4 | Configure PlatformIO for your board | ** |
| 5 | Writing microcontroller code in C | ** |
| 6 | Debugging and Printing | ** |
| 7 | Run a command shell | ** |
| &nbsp; | Total: | 100* |
<br>

\* - This lab is actually worth points - it is not a "free" lab!  Completion before lab 1 is required.  
  
\*\* - These steps are provided to show you the order of the lab and do not have associated points.  To earn full credit for this lab, you must submit your work to Gradescope and have soldered pin headers to your development board **before the beginning of your lab section for lab 1**.

## Instructional Objectives
- To become familiar with the BHEE 162 laboratory, policies, lab stations, and equipment.
- To configure your GitHub account for use with GitHub Classroom and add SSH keys for easy access.
- To get hands-on experience with the Proton development board, its accompanying components, and PlatformIO IDE in VScode that you will use all semester.

> [!CAUTION]
> HALT!  
>   
> This is an example of a note designed to get your attention. If you see this, read it carefully.  
>   
> **Make sure that you read this document in full. There are two ways in this lab to destroy your development board before you've even had a chance to use it.**  

## Welcome to ECE 36200!

In ECE 36200, you will learn fundamentals of microcontrollers, including their operation, and usage. In addition to the lecture portion of the course, students are expected to complete a series of lab experiments and a course project using a microcontroller and the supporting IDE.  

Computers and measurement equipment are provided in the course laboratory facilities to assist students in completing labs, however, **this course is designed to allow you to perform experiment exercises on their own computers at home or elsewhere, and we require that you make the most of this**.  The instructions provided in this lab document will guide you in setting up the microcontroller development environment used in ECE 362 to provide a consistent user experience between home and the laboratory.

## Introduction

Lab 0 will have you set up your brand new Proton development board that you should have received at the beginning of lab.  If you have not yet received it, please ask a TA or lab coordinator.

If you are in-person in lab and at any point you need help, you can add yourself to the lab queue via [QueUp](https://engineering.purdue.edu/~menon18/queup/?room=36200).  TAs in lab monitor this queue and will come to the station number you provide.

> [!IMPORTANT]
> In ECE 36200, labs are designed to be started **at home**, when the lab is released online.  This means that you must set up **both** your lab machine, and your personal computer, to do the labs.

## Lab Policies

> [!CAUTION]
> Please keep in mind the food-and-liquids policy of the lab, which is to bring absolutely no food or liquid with you to your lab sessions.  **Failure to follow this rule will result in a penalty for the lab currently running in that week.**  
>   
> This rule applies no matter what is happening in lab, be it your lab session or an open lab session.  If you must bring a bottle or other such container, please place it in the designated safe area for the lab.  If you must have food or drink, please step outside the lab to consume it.  **This rule is in place to protect the very expensive equipment in lab, and to ensure that the lab remains a safe and clean environment for everyone.**

> [!WARNING]
> Past semesters of students' breadboards have had numerous issues with tombstoned holes, which are holes that have been damaged by excessive pressure, typically from pushing in oscilloscope probes, which you may have done in prior classes.  If you have done this, or otherwise have had issues with your breadboard in the past, **we highly encourage you to replace it as soon as possible**.  Because the labs will build on each other, it will be extremely difficult for you to debug problems with your breadboard as you get to later labs.  A good replacement is the Makeronics breadboard (available for about $35 on Amazon).  
> 
> Debugging in this course can get very difficult with more complex circuits, so starting early is crucial to success in this course.  Do not put things off until the last minute - if you have breadboard issues, our recommendation is just to replace it, because **it will save you a lot of time and frustration in the long run**.

## Step 0: Initialize your git repository for your lab at home

In this course, you are required to use `git` to keep track of your code.  When you start work on a lab, you will **accept** a GitHub Classroom (abbreviated GHC) assignment that will give you your own private GitHub repository to work in.  You should work on each lab by backing up your work to this repository, allowing you to access it from either your personal machine or a lab machine.  In general, you'll follow these steps:

1. Starting at home, clone the newly created GHC repository to your local computer (which may be blank). 
2. Inside the cloned repository folder, create a project for your Proton board using the relevant VScode extension.
3. For each step, follow the lab instructions to write code and test it on your Proton board, and then add, commit and push your code to your repository.  The message must contain the step number you worked on.
    - Commits are how we will determine your effort on each lab assignment.  **You must commit your code for each lab to your respective git repository at each step of the lab, and not just one big commit at the end of the lab.** 
4. When you are done with the whole lab, submit your repository to Gradescope.  **You must submit your work to Gradescope before the beginning of your lab section next week.**
5. When you come to your lab section to get your work checked off, you can bring your personal machine, or you can clone your repository to a lab machine and show your work there.  **You must always show your work to a TA in lab or office hours to get credit for the lab**, unless you have been instructed otherwise.

To familiarize yourselves with git, keep this [cheat sheet](https://training.github.com/downloads/github-git-cheat-sheet.pdf) bookmarked.  After reading that, you should know how to:
- Create a new repository;
- Clone an existing repository to your computer;
- Add changes, commit them to your local repository, and push them to the remote repository;

Work for this class can be quite time-consuming, and so you need to **start your labs at home, finish as much as you can, and bring your work in to be checked off in lab**.  If you attempt to do the labs entirely in lab, you will have a very hard time completing them.  **Start early, and from home.**

Regardless of whether you are working on your own Windows/macOS/Linux computer or on a lab machine, you will be able to **clone** this repository to your own computer.  Make sure to do this on **all computers** on which you will be doing your lab.  

> [!TIP]
For speed and ease of use, we recommend learning to use SSH and public/private keys.  Generate your keys by following [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).  Once you have your key, read [this](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to learn how to add keys to your account.
> 
> Another option is authenticating the GitHub Extension with VScode with your account, allowing you to clone private repositories.  This will allow you to clone repositories without needing to enter your username and password each time.  You can do this by following the instructions [here](https://code.visualstudio.com/docs/editor/github).

If you haven't already, accept the Lab 0 GitHub Classroom assignment, and clone the repository to your computer.  You should see a README.md file in the repository.  If you don't, you may have cloned the wrong repository.  Make sure to clone the repository that has the lab-0-intro-username format.

> [!TIP]
> The link to each lab is posted on Piazza along with the link to a corresponding GitHub Classroom assignment.  You should accept that assignment, and clone the repository to your computer/lab machine.  Your repository will be located at some URL that looks like https://github.com/ece362-purdue/lab-0-intro-username, where `username` is your GitHub username.  You will be committing your code to this repository for this lab.  Each lab will have their own repositories and lab manuals that will be posted ahead of time.
> 
> By default, the lab-0 repository should only have a README.  Make a change like adding your name to it or creating a new file, and then add, commit and push the change to your lab-0-intro-username repository.

For each step that follows, **always remember to commit and push before you stop working and need to head to lab.**  The most basic commands to do this are:

```bash
# make a simple change, like creating a file first.  And then run:
git add .
git commit -m "A descriptive commit message"
git push
```

If you are on Windows, it is possible to use PowerShell to do this.  If you are on macOS or Linux, you can use a terminal.  `cd` (change directory) to the downloaded folder, and run these commands.  Make sure to change the commit message to include the step number, and a brief description of what you did in that step.   Our intention here is to get you used to the command line and git.

> [!IMPORTANT]
> Create a new file called hello.txt in your repository, and add some text to it.  Commit and push this change to your repository.  Use a descriptive commit message, eg. "Step 0 commit".

## Step 1: Solder pin headers to your development board

**Soldering!?  In ECE 36200?  What is this, ECE 20008?**

No, this isn't a typo.  In ECE 36200, you will be using a development board that has no pin headers soldered to it.  One of our two reasons is to eliminate the labor cost associated in soldering the pin headers to your board.  

The other reason is to start teaching you how to solder **before** you go into senior design.  When you are working with a microcontroller, there are numerous components that you will need to solder to your board, and forming the skillset necessary to soldering and desoldering components is a critical skill to have.  

In this lab, we are starting out with the most common component that you will need to solder - pin headers.  When you start doing projects, and you need to use external components that come on PCBs without headers, you will need to add them on before you can attach them to your breadboard, for both ECE 36200 and your senior design project.

Open [this page](soldering.md) in a new tab and follow the instructions.  

> [!CAUTION]
> 
> **Soldering is a skill that takes time to learn.  If you have never soldered before, please ask a TA for help.  If you have soldered before, please still ask a TA for help.  We want to make sure that you are doing it correctly.**

If you want to solder your headers outside of lab, you may do so, but make sure to get it done as soon as possible.  You will need to have your board in place on your breadboard, wired up, and running some test code before you start lab 1.

## Step 2: Place the Development Board on a Breadboard

Once you have soldered headers and placed the shorting plugs on your Proton development board, place it back on the breadboard as shown.

<div class="center">
    <img src="images/bb-layout.jpg" style="margin: 1em 0; width: 20vw; max-width: 400px">
</div>

Typically, a microcontroller will be a little hard to get into (and out of) the holes, but you should not have to remove it once it's in there for the rest of the course.  To prevent damage to the board, press down on both ends of the board at once. Do not apply force to anything else, as parts of the board are delicate and may become damaged. When inserting, make sure there are two columns on either side of the development board to leave room to plug in wires. 

Next, connect two wires: one from each of the 3V3 pins to the power rails, and one from each of the GND pins to the ground rails of your breadboard, as well as the PGND pins on the debugger.  Use the labels on the pins, but if they're too small for you to read, you can also use this [pinout diagram](https://ece362-purdue.github.io/proton-labs/assets/Proton%20Pinout%20Diagram.pdf).

Plug in a USB-C cable from the **debugger** - not the Proton - to your computer.  If the red LED on it doesn't turn on, there are two possibilities:

- It is a brand new debugger with no program saved in flash, in which case it will appear like a flash drive on your computer labeled "RPI-RP2".  Download the latest **debugprobe.uf2** firmware [here](https://github.com/raspberrypi/debugprobe) from the Releases tab, and drag the file into the drive.  This will flash the firmware to the debugger, and the red LED should turn on indicating it was recognized by your computer.

- If no flash drive appears when you plug it in, you have a USB-C cable that is only capable of carrying power, not data.  This is common with a lot of cables.  Try a different USB-C cable, or try a different computer to confirm that it is the cable that is the issue.  If you are using a lab machine, make sure to use a USB port on the back of the machine, as the front ports may not be able to provide enough power to the debugger.

Once your debugger's red LED turns on, plug in a second USB-C cable into the Proton board.  You should always see the 5V red indicator and 3.3V green indicator LEDs on the Proton board turn on.  If they do not, immediately pull out the USB-C cable - **there could be a short with how you connected it to your breadboard**.  More likely than not, though, the LEDs are probably just damaged, but ask a TA to confirm.  

If you see "RP2350" appear as a flash drive on your computer when it is connected, your board is fine, and it's just in bootloader mode.  You *could* drop in a UF2 file to flash it, but we don't have a compiled program just yet, so there's no need.  You may also see a green LED in the middle of the board start flashing, in which case your board was already tested and confirmed as working, and you can ignore it.

> [!IMPORTANT]
> In ECE 36200, you will build upon the existing circuit on the breadboard for each lab, so **do not take parts off your breadboard when you are done with a lab** - including this one!
> 
> Show a TA your setup before moving on to the next step.  **Do not proceed until you have shown a TA your setup.**
> 
> For every lab, ensure that you have received your checkoffs by checking this page: https://engineering.purdue.edu/ece362/checkoff/

## Step 3: Install VScode and PlatformIO

The way microcontrollers work is by writing a C program, compiling it into a binary file to be run by the specific CPU core on the microcontroller, and then flashing/uploading that binary file into the flash memory on the microcontroller.  When your microcontroller powers on, it loads that program from flash memory.

The method of flashing can be different, however.  A newly fabricated board will obviously have no data in flash memory, which the microcontroller can detect.  If there's no data, the microcontroller will enter **boot mode**, allowing you to upload a new program.  However, to upload a new program, it can be tedious to put the microcontroller into boot mode and then upload a new program over and over again.

Therefore, we have the second part of your Proton board - the debugger.  The debugger is actually **another microcontroller** that is connected to the Proton board, electrically and digitally separate from the Proton.  The debugger allows us to tap into the microcontroller's **debug interface**, which gives us the option to flash a new program to the microcontroller without having to put it into boot mode.  This is done by connecting the debugger to the Proton board using the SWD and SWCLK pins, which you already connected using the shunt jumpers on the top of your board.  The debugger can also be used to step through code, inspect variables, and set breakpoints in your code.

As mentioned above, these labs are entirely doable at home as they are in lab.  For applications that require looking at an oscilloscope, you may want to use an AD2 from the ECE shop in its place at home.  Make it a habit to look ahead at labs so that you know when you may need one.  

**In this lab**, we'll teach you how to install PlatformIO for Visual Studio Code (VScode) that will allow you to write, compile, and debug code for your microcontroller.  We'll cover how to create a project from scratch, although in subsequent labs we'll provide you with the project.  We'll also show you how to use the debugger to step through your code, and how to use the serial monitor to interact with your microcontroller.

If you haven't already used it for a prior class, download Visual Studio Code (commonly referred to as VScode) from https://code.visualstudio.com/.  

Once you have downloaded and installed VScode, open it up and click on the Extensions icon on the left side of the window.  Search for PlatformIO and install the Raspberry Pi Pico Extension.  Once you have installed the extension, reload the VScode window so that the extension starts setting itself up.  Once it's done, click the newly added PlatformIO logo in the left sidebar.

![pico-ext-install](pico-ext-install.gif)

If it shows the sidebar, it should be good to go!  If it doesn't, you may need to restart VScode.  If you still have issues, please ask a TA for help.

> [!IMPORTANT]
> Show your newly installed VScode and Pico Extension to a TA.  

## Step 4: Configure PlatformIO for your board

In PlatformIO sidebar that appears, click New C/C++ Project.  In the window that appears, do the following:

- Specify `lab0` as the name.
- Select `Proton` as the board.
- Set the project location to be `lab0` in your respective `lab-0-intro-username` repository.
    - This will allow you to commit and push your newly created project to be submitted.
- Tick the box for RISC-V architecture.
    - Feel free to try out assembly code from your lectures (when you get to it) on this board!
- Set the Pico SDK version to the highest value.
    - As of 2025, the extension is being constantly updated, so the highest version will change constantly.  v2 or higher supports the Proton.
- **Under Stdio (standard I/O) support, tick `Console over UART`.**
    - You should always select this feature when creating a new project, as it will allow you to use the serial monitor to interact with your microcontroller.
    - Don't forget to do this, or you won't be able to see any output from your Proton via the terminal!
- Leave all the other options at their defaults.

Finally, click `Create`.  The extension will close, and you'll see a new window appear.  

The first time you create a project, it may take a while to set up the project, so **be patient**.  If it's slow, you can ensure it's working by watching the progress at the bottom of the VScode window where you created the project.  Once the new window appears, you can close the old window.  

If any errors appear, you may need to restart VScode, and/or try to reinstall PlatformIO.  If you still have issues, please ask a TA for help.

> [!IMPORTANT]
> Commit the newly created project and push it to your repository now.  Use a descriptive commit message, eg. Step 3 new project.

## Step 5: First Flashes

Now that we have a project, let's inspect the C code.  Open the `lab0.c` file.

We have some starter code that will initialize stdio (standard input/output) over UART, which will allow us to interact with the microcontroller using a serial monitor, and then print `Hello, world!` every second infinitely to the monitor.

The UART is an example of a **peripheral**, which is an independent subsystem of your microcontroller, separate from the microprocessor, that can be configured to autonomously perform some operation without the CPU's intervention.  In this case, the UART is configured to automatically send and receive data over a serial connection, which is a common way to communicate with a microcontroller.  By the end of the course, you will learn how to configure this yourself, but for now, we'll use the provided code.

Let's add some code that will also flash the onboard LED every second.  There is an LED on GPIO25 that we can use for this purpose.  After the `stdio_init_all` line, add the following code:

```c
    gpio_init(25);
    gpio_set_dir(25, GPIO_OUT);
```

And inside the `while` loop that prints `Hello, world!`, add the following underneath the `printf`:

```c
    gpio_put(25, 1);
    sleep_ms(250);
    gpio_put(25, 0);
    sleep_ms(250);
```

If you have your USB plugged into your Pico, disconnect it first, and then hold down the button labeled `BOOTSEL` on the board, and plug the USB back in.  This will put the Pico into a mode where it can be flashed.  If you're on Windows/macOS, you may see a pop-up appear saying there's a new drive.  This is normal, and is a way for you to drag UF2 files to flash the Pico with a new program.  However, we won't do that - we'll let VScode handle it for us.

Go to the Pico menu and click `Run Project (USB)`.  **When your Proton has no program flashed on it, or is otherwise in bootloader mode (done by holding down BOOSEL and clicking the reset button or applying power), you need to click this first.**  This will build your code using the CMake build system (not the same as `make`) and flash it to your microcontroller.  If you see a textbox appear asking about CMakeLists.txt, you can hit Escape to get rid of it.  You should now see your LED flash twice every second (just so you can tell it's different from the factory condition).

Hold down the reset button, and you'll see that the Pico does nothing in this state.  The button is connected to the RUN pin on your board, which is tied to 3.3V high internally, and shorting it to ground resets the microcontroller.  Let go, and you'll see it run again.

We'll figure out how to see the "Hello, world" text in the next step (although with microcontrollers, a flashing LED is equivalent to a "Hello World").

> [!IMPORTANT]
> Show your TA your blinking LED.  Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number.

## Step 6: Debugging and Printing

The USB cable that we just flashed the Pico with is only useful for providing power and flashing the Pico.  However, there are two critical things we need to be able to use ~~the Pico~~ any microcontroller to its fullest potential:

1. **Debugging** - the ability to step through code and inspect variables as the program runs on the microprocessor.

2. **Serial Communication** - the ability to send and receive data from the microcontroller to a computer.

In your kit, there is a second item called the Raspberry Pi Debug Probe that will help us with these things.  Go ahead and pull this out and wire it up as per the instructions [here](https://www.raspberrypi.com/documentation/microcontrollers/debug-probe.html) to your Pico, and plug it into your PC.

There are six important pins to connect on your Pico:
- GND (ground) to GND on the Debug Probe.
    - When you are interfacing two digital systems together, you must connect their grounds together to ensure that they have a common reference point.
- The TX pin of the Debug Probe to the RX pin of the Pico.
    - This is the transmit pin of the Debug Probe, which sends data to the Pico.  When you "type" into the Serial Monitor, the data is sent to the Debug Probe, which sends it to its UART TX pin, which is connected to the Pico's RX pin.
- The RX pin of the Debug Probe to the TX pin of the Pico.
    - This is the receive pin of the Debug Probe, which receives data from the Pico.  When the Pico sends data to the Debug Probe, it sends it to its UART RX pin, which is connected to the Pico's TX pin.
- SWDIO and SWCLK to the SWDIO and SWCLK pins on the Pico.
    - These are the Serial Wire Debug (SWD) pins, which are used to communicate with the microcontroller's debug interface.  This is how we will be able to step through code and inspect variables.  We can also use this to flash a new program to the Pico without having to hold down the BOOTSEL button.

Once you're all connected, go back to VScode and click `Flash Project (SWD)` option.  You should see a recompilation and flash process using OpenOCD, which is a tool that talks to the Debug Probe to send the program to be flashed to your Pico.  You should see something like this in the case of a successful flash:

```
Info : [rp2350.dap.core1] Examined RISC-V core
Info : [rp2350.dap.core1]  XLEN=32, misa=0x40901105
Info : [rp2350.dap.core1] Examination succeed
Info : starting gdb server for rp2350.dap.core0 on 3333
Info : Listening on port 3333 for gdb connections
** Programming Started **
Info : RP2040 Flash Probe: 33554432 bytes @0x10000000, in 8192 sectors

Info : Padding image section 2 at 0x10002264 with 156 bytes (bank write end alignment)
Warn : Adding extra erase range, 0x10002300 .. 0x10002fff       
** Programming Finished **
** Verify Started **
** Verified OK **
** Resetting Target **
shutdown command invoked
```

If you see this, you have successfully flashed your Pico with the new program, and you should see the LED flashing twice a second again.  

To set up the Serial Monitor, press Ctrl-Shift-P (or Cmd-Shift-P on a Mac) and type `Terminal: Focus on Serial Monitor View`.  You can also pull it up if you have a terminal open by looking for the "Serial Monitor" tab.  Open it, and click Start Monitoring.  You should see the "Hello, world!" message appear every half second.

If you are monitoring but you see no messages, verify that:
- A light is blinking at least every second on your Debug Probe without any intervention.  This verifies that text is being sent to your computer through it from the Proton.
- You are connected to the correct Port.  Some computers may have virtual COM port devices, so you may be connected to the wrong one.  Try changing the Port options to see if the message appears.

![serial-monitor](serial-monitor.png)

Now, let's test debugging.  Before we just click Debug, we should make some changes to the CMakeLists.txt file in the project to ensure that all optimizations are turned off so that we can step through every line of our code without the compiler optimizing it away.  Open the `CMakeLists.txt` file in the project, and below the line that says `project(lab0 C CXX ASM)`, add the following lines:

```cmake
set(CMAKE_BUILD_TYPE Debug)
set(PICO_DEOPTIMIZED_DEBUG 1)
```

Next, set a breakpoint on the `printf` line by clicking in the left margin of the editor window - you'll see a red dot appear.  Then, click `Debug Project` in the Pico menu.  Select `Pico Debug (Cortex-Debug)` as the method of debugging.  This will start OpenOCD to connect through the Debug Probe and flash a program, but **not exit**, and start `gdb` to connect to the running OpenOCD process to control line-by-line debugging.  You should see the program stop at the first line of `main`.  Here is a picture of the window as you should see it:

![debugger](debug-view.png)

There are a lot of things going on in Debug mode that you should understand:

- First, in the top right is a terminal with the OpenOCD process.  When you clicked Debug, it sent your compiled code to be flashed on to your Proton through the Debug Probe.  Instead of "shutting down" as it did earlier, it is now "halted due to debug-request".  Your Proton is dual-core, but your code will only run on one by default.
- The Debug Console shows output from `gdb`, which may be familiar to you from prior coding classes.  You have used `gdb` in the past to debug programs as you ran them on your own computer's CPU, but now you are using it to debug a program running on a completely separate computer - your microcontroller.  This is called "remote debugging".
- On the left sidebar, you have the `Variables` tab.  When you define variables, you can see their values here.  
- The `Call Stack` tab shows you the current function call stack.  This is useful for understanding how you got to where you are in your code.  In a dual-core configuration, you can see the call stack for both cores.
- The `Breakpoints` tab shows you all the breakpoints you have set in your code.  You can enable and disable them here.
- The `XPeripherals` tab (not `Peripherals`) shows you the state of the peripherals on your microcontroller.  As you start configuring them in next labs, we'll have you look here to see their state and understand how they work.

In the center-top of your window, you'll see a toolbar with the following buttons:

- `Reset device` - this will reset your microcontroller.  You can use this to restart your program from the beginning.
- `Continue` - this will continue running your program until the next breakpoint.
- `Pause` - this will pause your program if it's running.
- `Step over` - this will run the next line of code, but not step into any functions.
- `Step into` - this will run the next line of code, and step into any functions.
- `Step out` - this will run the rest of the current function and stop at the next line of the calling function.
- `Restart` - this will restart your program from the beginning, similar to `Reset device`.
- `Stop` - this will stop your program and disconnect the debugger, at which your Proton will remain halted.

> [!IMPORTANT]
> Show your TA your working serial connection and debug mode.  Show that you can step over the `printf` lines and the LED output value change lines, and that you can see the Proton LED turn on you step over the `gpio_put(25, 1)` line.  **Do not proceed until you have shown a TA your working debugger.**  Commit all your changes and push it to your repository now.  Use a descriptive commit message that mentions the step number.

## Step 7: Run a command shell

In your upcoming labs, you will include a code object that gets built along with your C code, typically called "autotest.o".  This provides (in other labs) an autotester that you can use to test individual functions and generate a confirmation code that you will submit with your code.  In this lab, it's simply a shell that executes some functions - just to get you familiar with how it works so you know how to use it for the following labs.  

In this step, we will have you add the autotest file to your PlatformIO project.  In the other labs, we will provide you with PlatformIO project with the autotester included.

Download the `autotest.o` file [here](../../../raw/main/lab0-intro/autotest.o) and place it in the root directory of your project, alongside the C file.

> [!NOTE]
> To clarify, this is not a real autotester.  It is not responsible for giving you any credit on the lab.  This is just provided to show you how you can use it in future labs.
> 
> Credit for this lab is granted by showing your TA that you have completed all steps in lab or office hours, by pushing your code to the GitHub repository, and submitting your GitHub repository to Gradescope.  **You must submit your work to Gradescope before the beginning of your lab section next week.**

To include the autotester in your project, open the `CMakeLists.txt` file in your project, and change the `add_executable` line as follows:

```cmake
add_executable(lab0 lab0.c ${CMAKE_CURRENT_SOURCE_DIR}/autotest.o)
```

You can also make a dynamic target that automatically includes any .o or .c files in the project directory as follows. 

```cmake
file(GLOB SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/*.c ${CMAKE_CURRENT_SOURCE_DIR}/*.o)
add_executable(lab0 ${SOURCES})
```

In your `main.c` file, add `autotest();` as a new line at the top of your `main` function.  

Flash this to your microcontroller, and reopen the Serial Terminal as you did in the prior step.  You may see nothing at first - this is normal.  By the time you pull up the serial monitor after the microcontroller has been flashed, the autotester will have already printed text that you did not see.  To see this text, you can press the reset button on your microcontroller, and you should see a "command shell" appear.  

To be able to **type** into the Serial Monitor, you have to switch it to Terminal Mode - unfortunately, the default is to type into the long textbox at the bottom and then hit Enter, which in our opinion is not how these things should work.  In the same row as "Start Monitoring", there is an icon with a ">" symbol that indicates Terminal Mode.  Make sure that's enabled before you start typing into the terminal window.

![terminal.png](terminal.png)

This "command shell" is where you will type commands to execute code on your microcontroller.  For the rest of your labs, we'll give you commands to test individual functions, but for now, type 'help' to see what commands are available.  You can type 'exit' to leave the shell.

By the end of the embedded systems labs, we'll give you instructions on how to write your own command shell.

> [!IMPORTANT]
> Show your TA your working autotester.
> Commit all your code and push it to your repository now.  Use a descriptive commit message that mentions the step number.  Show your TA that you have been pushing commits for each step.

## Sign Your Breadboard

If you haven't done so already, your TA will hand you a silver sharpie that so that you can write your username at the top of the breadboard.  They will then sign it themselves.  If the breadboard was used by someone else whose username is on it, cross out the previous username. 

## Submit your work to Gradescope

Submit your work to the Lab 0 assignment on Gradescope **before the beginning of your lab section next week**.  Ensure that your PlatformIO code has been added and pushed in your GitHub repository.  

## Lab Station Clean-up

At this point, you are free to leave. Please make sure to close all windows, log out of the machine (click on your name on the top bar, and click Log Out, or similar), take all your belongings with you, and exit the lab. You must perform this clean-up procedure in every lab, or you will lose all points on the lab. 

> [!IMPORTANT]
> You must only leave after you have gotten your breadboard signed and you have cleaned up your station.  Do not leave behind plastic wrappers, breaboard wires, or any other trash as a result of setting up your breadboard in lab.  **Failure to clean up your station will result in a penalty for the lab currently running in that week.**
