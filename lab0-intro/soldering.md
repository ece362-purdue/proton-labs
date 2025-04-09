# Soldering pin headers to your Proton board

## Tools and materials needed
- Your brand new Purdue Proton board
- Two 60-pin pin headers
- Your breadboard
- Soldering iron
- Solder
- Flux

> [!CAUTION]
> **Safety first!**
> - Ensure the soldering is turned OFF. 
> - Ensure the fume hood is turned ON so that it pulls fumes away from your face.
> - Ensure the soldering iron is in its holder.

## Introduction

You should have your components in front of you, as shown below.

<div class="center">
    <img src="images/soldering-setup.jpg" style="margin: 1em 0; width: 20vw; max-width: 500px">
</div>

Take one of the row header and break it as follows:
- 1 4-pin header
    - This will be used as the debug and boot pins on the Proton debugger.
- 3 3-pin headers
    - One will be used as the UART pins on the Proton debugger.
    - The other two will be used to connect the debugger to the Proton board, using the 2x3 through-hole pads (holes) on both sides of the line joining the Proton board and the debugger.
- 1 28-pin header
    - This will populate the holes on the Proton board on one side.

Then, take the other row header and break off 56 pins so that it goes in on the other side of the Proton board.  Use the board itself to help you properly count the pins.

> [!TIP]
> If you accidentally break off fewer pins, you can still solder on the smaller piece and add the extra pins separately.

Next, place the headers into the breadboard in the positions indicated.  With the head of the breadboard (where the title and voltage plug holders are) turned left, you will (eventually) place the Proton board on the third row from the top with the top of the Proton board facing outward.  

For now, however, we'll put down the pin headers in their place instead, using the breadboard to ensure proper alignment.  Note how the board is now aligned with the pin header layout as shown.

<div class="center">
    <img src="images/soldering-alignment.png" style="margin: 1em 0; width: 40vw; max-width: 500px">
</div>
<br>

We'll solder these last.

## Soldering the debug pins

Now, take two of the 3-pin headers, and place them into the Proton board in the 2x3 pin positions as shown below, but **upside down**, and insert the pins into the breadboard so that the board sits upside down on them.  This will allow us to more easily hold the board in place as we start soldering the 2x3 header.  The pictures below show how to do this.  Make sure the longer side goes in the breadboard, and the shorter side sticks up above the board, as shown in the last image.

<div class="center">
    <img src="images/soldering-debug-pins-1.png" style="margin: 1em 0; width: 25vw; max-width: 500px">
    <img src="images/soldering-debug-pins-2.png" style="margin: 1em 0; width: 25vw; max-width: 500px">
    <img src="images/soldering-debug-pins-3.png" style="margin: 1em 0; width: 25vw; max-width: 500px">
    <img src="images/soldering-debug-pins-4.png" style="margin: 1em 0; width: 25vw; max-width: 500px">
    <img src="images/soldering-debug-pins-5.png" style="margin: 1em 0; width: 25vw; max-width: 500px">
</div>

Now, let's get to soldering!  Grab your flux pen - it'll be a white pen with a yellow cap - pull off the cap, and dab the pins with flux as shown below.  The flux ensures proper heat dissipation and allows the melted solder to properly wet the PCB pads and the pins.  This is very important, as it will help the solder to flow properly and create a good connection between the pin and the pad.

<div class="center">
    <img src="images/soldering-flux.gif" style="margin: 1em 0; max-width: 800px">
</div>

> [!WARNING]
> At this point, have a TA come over to supervise your work.

Turn on the soldering iron, and wait until the temperature reaches 350 F, or the knob is turned to 75% and until the LED starts blinking (you may have a different kind of soldering iron).

Once the desired temperature has been reached, unspool some solder and hold the solder such that the end of it - which is going to be **heated to a very high temperature!** - is at least an inch away from your fingers.

<div class="center">
    <img src="images/soldering-dp.png" style="margin: 1em 0; max-width: 800px">
</div>

**Read the paragraph below in full and watch the animation first before trying it.**

Touch the solder to the pin and pad, and move the iron to the same position such that it makes contact with the solder.  You'll have to angle how you press down on the board to ensure the pins are standing up straight and not at an angle.  Touch the iron to the solder, and the second you see the solder melting, **pull the iron back** and look at the connection formed between the pin and the pad.  

<div class="center">
    <img src="images/soldering-first.gif" style="margin: 1em 0; max-width: 800px">
</div>

Note how the solder "fills up" the gap between the pin and the pad.  

Compare your first soldering job against this diagram to determine if you used too much.  A bit more solder on the pin is fine as long as it won't bridge to another pin.  If you have a hole in the solder joint, you can reheat it and add more solder.

<div class="center">
    <img src="images/soldering-joint-checks.png" style="margin: 1em 0; max-width: 800px">
</div>

The first soldered pin is critical because at this stage, you can correct any mistakes such as the positioning of the board.  **If the pins are soldered at the wrong angle, you will find it very hard to connect jumpers between them later on.**  If you feel that your board is at an angle, have a TA confirm, hold only the iron to the soldered pin to heat it up, and when it melts, adjust the board, and take off the iron.

If the pins are okay, continue soldering the rest of the pins.  At the end, they should look like this:

<div class="center">
    <img src="images/soldering-joints-all.png" style="margin: 1em 0; width: 400px">
    <img src="images/soldering-joints-all-side.png" style="margin: 1em 0; width: 400px">
</div>