# Altair Languages

## General Instructions

These .hex files can be loaded on the Arduino-based Altair clone using the built in debugger using the approach described below.

* From power up, hit STOP and then RESET.
* Hold STOP+AUX1(UP) to enter the setup menu.
* From the terminal, enter "i" to enable input, "d" to enable debugging, and then 'x' to exit setup and dump into the debugger. You will see the PC (program counter), SP (stack pointer), registers, and such displayed, but you can ignore that.
* Next you will need to loade the .hex data which is contained in this directory to your clipboard.
* I used Tera Term and set the transmit delay to 10 milliseconds per character.
* You should still be in the monitor, so if you type 'H' you should see the message, "Reading HEX data...".
* In Tera Term, go Edit and then Paste. This will give you a preview window and the option to click okay and begin the transfer to the Altair clone.
* You will see the results of each line of hex data as it loads.
* Once the hex data has been loaded, you will be dumped back into the debugger.
* Now type '!' for a hard reset (the same as STOP+RESET from the front panel). This will, among other things, reset the PC (program counter) to 0. You will get the status dump again also.
* Now type 'r' (it must be lower case) to begin running things (from the PC which we reset to 0 with the last command).
* At this point, the terminal should should any startup messages associated with the language you are running as well as the prompt. There may also be questions such as memory size or terminal width to answer.

## Palo Alto Tiny BASIC 1.0

The files associated with this originally came from:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/

David Hansel figured out that the original code expected Rev 0 of the 88-SIO card (and not the 88-2SIO as I originally suspected) while the clone emulates the more common Rev 1.

David identified two patches necessary, one for sending and one for receiving.
* At address 0723, the "JZ" needs to be changed to "JNZ".
* At address 0734, the "NOP; ANI 20H" needs to be changed to "CMA; ANI 01H".

I have cleaned up the .hex file and changed to last few lines using his suggestion. The .hex file should load and run without further modification.

The original files are contained in the appropriate subdirectory.

## Palo Alto Tiny BASIC 2.0

The files associated with this originally came from:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/

I have cleaned up the .hex file and it should load and run without further modification.

The original files are contained in the appropriate subdirectory.

## XYBASIC

XYBASIC was a commercial product that has been resurected and released for non-commercial use. The original .hex file found here worked fine as-is:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/xybasic.hex

More info on XYBASIC is at:

http://www.nesssoftware.com/home/mwc/XYBASIC.php

This is a very complete and powerful implementation of BASIC.

