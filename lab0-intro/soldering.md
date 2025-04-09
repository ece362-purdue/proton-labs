# Soldering pin headers to your Proton board

## Tools and materials needed
- Your brand new Purdue Proton board
- Two double-sided pin headers (both sides are almost the same in length)
- One single-sided pin header
- Your breadboard
- Soldering iron
- Solder
- Flux

> [!CAUTION]
> **Safety first!**
> - Ensure the soldering iron is turned OFF. 
> - Ensure the fume hood is turned ON so that it pulls fumes away from your face, and placed over your breadboard.
> - Ensure the soldering iron is in its holder.

## Introduction

You should have your components in front of you, as shown below.

<div class="center">
    <img src="images/soldering-setup.jpg" style="margin: 1em 0; width: 80vw">
</div>

Take the single-sided pin header and break it as follows:
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

> [!WARNING]
**Read the paragraph below in full and watch the animation first before trying it.**

Touch the solder to the pin and pad, and move the iron to the same position such that it makes contact with the solder.  You'll have to angle how you press down on the board to ensure the pins are standing up straight and not at an angle.  Touch the iron to the solder, and the second you see the gap being plugged by the melted solder, **pull the iron back** and look at the connection formed between the pin and the pad.  

It might be that you touch the solder at a different angle and it ends up on the iron, facing upward.  In that case, just turn the iron and touch the melted solder ball on to the pad, which will have the same effect.  

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

## Soldering the main headers

Now, put the board back on the pin headers we laid out on the breadboard.  As a reminder, it should go in the third (from the top) row of your breadboard, with the USB-C connector on the short side of the board facing outward, and the debugger USB-C connector facing towards the top of the breadboard, as shown below.

<div class="center">
    <img src="images/bb-layout.jpg" style="margin: 1em 0; width: 80vw">
</div>

The board should sit flush against the pin headers, with all the pins going through the pads.  If it doesn't, take out the pin headers and fix the positioning accordingly.

Next, apply flux to all the pads on the board with your pen.  You only need about a dab per pin, or until you see the flux liquid appear around the pad.  The easiest way is to drag across the outer boundaries of the pads for an even flux distribution, which is fine since we'll be holding the solder from the outside anyway.

Starting with the side furthest from you, solder the leftmost 5V pin, and the GND pin on the end of the devboard.

<div class="center">
    <img src="images/solder-board-1.gif" style="margin: 1em 0; width: 45vw">
    <img src="images/solder-board-2.gif" style="margin: 1em 0; width: 45vw">
</div>

This method isn't quite necessary since we're using a breadboard, but it is good practice to solder the edge pins first to ensure that the pins are in the right position before you continue soldering the rest of the pins.  Look at the board from different angles to ensure that your board is still sitting flush, and that it is not raised at an angle.  If you make a mistake here, now is the time to fix it by reflowing the two pins with your soldering iron.

Next, we're going to try **drag-soldering** the remaining pins on this side of the board.  Hold the solder line against all the pins, with the end on the rightmost pin, take your iron, and touch the solder to the first pin.  As soon as the solder melts, start moving your iron to the left, pausing to let the solder melt on to each pad before you continue.

*This animation is sped up 4x to reduce file size.  Don't worry if you're not this fast - we weren't!*

> [!CAUTION]
> During this process, pay very close attention to where the metal part of the iron is, where your fingers are, and where the solder is.  

<div class="center">
    <img src="images/solder-board-3.gif" style="margin: 1em 0; width: 80vw">
</div>

Go ahead and do the same with the 4 pins on the debugger, and that should close up our first side of the board!

Flip your breadboard, and do the same thing with the other side, and the 3 pins on the debugger board.

By the end, your board should look like this.  If it does, <span style="color: gold">**congratulations!**</span>

<div class="center">
    <img src="images/solder-final.png" style="margin: 1em 0; width: 80vw">
</div>

## Cleaning the board after soldering

Now, we need to take the board out of the breadboard to clean it - a difficult task owing to the large number of pins on the board!  You will only have to do this **once** throughout the course.  

You'll need to **see-saw** the board out, which is to say that we'll slowly lift the board from both sides, while being extremely careful to not snap the board in the middle.  (Even if it does snap, it's not an issue, since we soldered on pin headers and can just connect them later.  But it's nice not to break it!)

*2x speed to reduce file size.*

<div class="center">
    <img src="images/bb-extraction.gif" style="margin: 1em 0; width: 80vw">
</div>

Spray the board with isopropyl alcohol, focusing on the soldered pins, and lightly scrub it with a metal brush.  This will remove the flux that we applied earlier, and any other contaminants that may have gotten on to the board during soldering.  The alcohol will evaporate quickly, so you don't need to worry about it damaging the board.  If there's still some wetness to the board, take a tissue and dab it off - don't rub.

*2x speed to reduce file size.*

<div class="center">
    <img src="images/brush-clean.gif" style="margin: 1em 0; width: 20vw">
</div>

## Adding the shorting plugs

Go ahead and put on the **shorting plugs** on your board's debugger pins.  The purpose of this 2x3 pin header is to allow you access to the **debugging interface** on your microcontroller, a very common feature in embedded systems development.  When your product goes out into the real world, the debugger is not normally attached, but since our primary focus is **learning** embedded systems, the debugger is critical to understanding what your code is, which is why it's part of your Proton devboard.  Your shorting plugs should go on between the devboard and debugger, like this:

<div class="center">
    <img src="images/debugger.jpg" style="margin: 1em 0; width: 40vw">
</div>

By adding the shorting plugs, you're connecting the debugger's SWCLK, GND and SWD pins to the devboard.  The SWCLK and SWD pins are akin to `clock` and `data` signals that you should recall seeing from ECE 27000, and are controlled by the debugger when it is attached to your PC via the USB-C connector.

## Admire your work!

Your board is now fully assembled.  Go back to the [lab0-intro](README.md) page to continue with the rest of the lab, and best of luck!