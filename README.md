# Propeller_DIPstick
Dev board for the propeller 1 microcontroller

The Propeller 1 by Parallax is an unusual, 8-"cog" (not quite core) microcontroller first released in 2006.  It has since been replaced by the Propeller 2, but for a few years I've had some of the delightfully chunky DIP-40 versions of the chips sitting in one of my drawers.  I've tried out the chips a couple of times, and while they're fun, the breaboards get quite busy: as well as the chip itself, it needs a 3v3 regulator (they aren't 5V tolerant), a USB serial converter to program it with, an EEPROM for storing programs, and a reset button.

There are official dev boards for the Propeller which include all of this, but they're relatively expensive and not that easy to get hold of where I am, and besides, why buy a board when I already have the processor?  So I designed my own, and this repo contains the files for it.

Setting out, I wanted this board to:
* Contain all of the necessary parts for a fully-functional Propeller: USB serial, 3v3 power, an EEPROM, and a reset button.
* Make use of the big, chunky DIP-40 chips which I already had, instead of requiring me to buy additional chips in a smaller format.
* Be as small as possible on the breadboard, to give plenty of space for LEDs, buttons etc.
* Use USB type C.  A lot of the official dev boards use micro B, get with the times!

# The Design

The schematics for the board are [in this repo](https://github.com/UnfinishedStuff/Propeller_DIPstick/blob/main/Propeller_DIPstick_schematic.pdf).  The major components are:  


| Component      | Function                |  Reason for choice                        |
|----------------|-------------------------|-------------------------------------------|
| Silicon Labs CP2102N-A02-GQFN24R | USB to serial converter | Allows serial connection and programming over a USB cable.  Readily available from major wholesalers, fairly cheap, [not made by FTDI](https://hackaday.com/2014/10/22/watch-that-windows-update-ftdi-drivers-are-killing-fake-chips/).|
| ON Semi NCP708MU330TAG  | 5v to 3v3 regulator    | The Propeller needs the 5V from the USB connection stepped down to 3v3.  Small footprint, only needs 2 capacitors to support it, and outputs an ample 1A of current, plenty for all of the dev board components and some LEDs on a breadboard. |
| Parallax P8X32A-D40  | Microcontroller | The whole point of the dev board, an interesting 8-cog microcontroller which does a few hardware things differently to most other microcontrollers. |
| Microchip 24LC256T-I/SN | EEPROM | Used to store programs between reboots (the Propeller's internal memory is volatile).  Cheap, widely available, fairly generic. |
| Panasonic EVQ-P7C01P | Button for resetting the Propeller | It's a reset button.  Small, convenient Kicad footprint! |
| Murata CSTCR5M00G53-R0 | 5 MHz resonator | Used to help the chip keep time properly.  Doesn't require additional capacitors like a crystal, small but still hand-solderable. |
| Adafruit 4458 USB Type C SMT / THM Jack Connector | USB connector | Physically a USB C connector, but wired for USB 2 connections so it has fewer pins than USB 3 connectors.  Has solderable lugs for physical support. |

To keep the board as compact as possible [the layout](https://github.com/UnfinishedStuff/Propeller_DIPstick/tree/main/Propeller_DIPstick_gerber) takes a stacked approach: most of the components are soldered directly to the PCB, and then a DIP40 socket is soldered over the top of this.  The DIP40 Propeller chip can then be inserted into the socket, keeping almost everything within the footprint of the P1 chip.  A set of 0.1" headers is then soldered just inside the footprint of the DIP socket so the whole board can be inserted into a breadboard.  This gives a board about the size of a stick of gum even though it uses the large DIP40 chip, so in keeping with the engine-themed names that Parallax has for the Propeller and its accessories, I decided to call this the DIPstick.

You can see this in the image below: the bottom PCB shows the upper side of the board, with most of the components in the narrow centre section.  The outer holes along the edge are for the DIP40 socket, and the inner rows are for the 0.1" headers.  The only components sticking out of this footprint are the USB C socket and some LEDs/resistors to the left.
![PCB layout](https://raw.githubusercontent.com/UnfinishedStuff/Propeller_DIPstick/main/images/PCB.jpg)

The upper image shows the underside of the PCB which doesn't have a lot on it, but does have a Bunny wearing a Propeller Hat.
![Bunny on PCB](https://user-images.githubusercontent.com/20929510/114467926-92a2f780-9be2-11eb-8cd8-24a1802221a9.png)

# The Board

The whole thing was hand-soldered, and came together quite nicely (ignoring some hardware flaws in v0.1...).  You can see the completed stack below, with the P1 chip removed in the image below that.

![Completed stack](https://raw.githubusercontent.com/UnfinishedStuff/Propeller_DIPstick/main/images/DIPstick1.jpg) ![Completed without the P1](https://raw.githubusercontent.com/UnfinishedStuff/Propeller_DIPstick/main/images/DIPstick2.jpg)

It's fairly chunky, but honestly works well.  I love the small footprint of the whole thing, despite the size of the P1 chip.  I'm not actually sure I could have made it any smaller even if I'd bought the smaller 44-pin QFP package, because that would have to be soldered directly to the PCB with the rest of the components, instead of being "stacked" over them via a socket.  The [Propeller Flip](https://www.parallax.com/product/propeller-flip-microcontroller-module/), the smallest official dev board, is 51x18 mm while the DIPstick is 59x18 mm, so there isn't a lot in it.

The pair of LEDs beside the USB socket are the TXT and RXT LEDs of the CP2102N, and show whether there is data being transmitted/received by the CP2102N.  It's worth noting that this function isn't enabled by default, you have to use Silicon Labs' Simplicity Studio to enable it.  It's fairly straightforward, although you do have to create an account to get it.

# v0.3?

Overall I'm happy with the board and it functions well.  The one thing I'd change about it is the reset button: it is poking out the underside of the rear of the PCB, and actually doesn't stick out more than a millimetre or so.  This actually makes it quite hard to push with a finger, you really need to use something to poke it.  For convenience I think it would be better to extend the board a handful of millimetres further and use a larger button to make it easier to press.  

![Button](https://raw.githubusercontent.com/UnfinishedStuff/Propeller_DIPstick/main/images/button.jpg)

That said, I'm happy enough with the whole thing that I'm not going to worry about a version 0.3.  The button issue won't get in the way of learning how to use the board, and in a worst case scenario the reset pin is broken out on the pin headers, so I could wire a reset button onto the breadboard.  I'll keep it in mind for the Propeller 2 DIPstick...
