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

## VTL-2

VTL-2 is the implementation of a Very Tiny Language for the Altair 8800.

Tom Lake made the port from a ROM image to a .hex file.

VTL stores a number of system variable in different symbols. The documentation recommends that after loading and getting the 'OK' message, you set the memory size using the command "\*=1024" for a system with 1K of memory. The Altair clone has 64K of memory and VTL-2 itself is stored at $F800 which leaves 62K of memory free. So for our system, we can set the upper memory using the command "\*=1024\*62". (Which lets VTL do the math for us...)

You also need to set the lower memory boundary. VTL-2 has a number of variable and a buffer stored in lower memory, so the actual start of Free Memory available for our programs starts at $0140 (or 320 decimal). We tell the system this with the command "&=320". This variable is adjusted as you type in your program to reflect the current start of Free Memory.

As you use VTL-2, if you want to do the equivalent of BASIC's NEW command, you simply reset the start of available memory by using that same command.

To determine the amount of free memory (which really should never become an issue on the clone), you can type "?=\*-&" as a direct command.

I have modified the .hex file with three entries at the end. These set the upper memory limit, the lower free memory, and put a jump to VTL-2 at $0000 (JMP $F800 or C3 00 F8). This should allow you to load and run VTL-2 using the same instructions initially described and not have to set the memory parameters. You can verify that memory has been properly allocated with the commands "?=\*" and "?=&".

## XYBASIC

XYBASIC was a commercial product that has been resurected and released for non-commercial use. The original .hex file found here worked fine as-is:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/xybasic.hex

More info on XYBASIC is at:

http://www.nesssoftware.com/home/mwc/XYBASIC.php

This is a very complete and powerful implementation of BASIC.

## Credit where credit is due...

Thanks to Mike Douglas at:

http://altairclone.com/

Mike has gathered an incredible amount of Altair material and I suspect that some of what floats around unattributed can often be traced back to his efforts. The Altair 8800 clone he offers is the "gold standard" among vintage computer clones.

Thanks to Chris Davis at:

https://www.altairduino.com/

His Arduino-based Altair 8800 kit is affordable and easy to build, making the fun of the Altair available to most who have an interest.

Thanks to Li-Chen Wang for the original Palo Alto Tiny Basic and the foundation of sharing that he created with his "COPYLEFT" and "ALL WRONGS RESERVED" quips.

I had a little trouble definatively determining who first wrote VTL, but thanks to Frank McCoy and Gary Shannon along with others who have built on it and kept it alive.

Thanks to Stephen A. Ness, author of XYBASIC, and to Robert Swartz, the current copyright holder who allowed it to be offered as an open source project. Also to Emmanuel Roche, Steven Hirsch, and Udo Munk for bringing it back to life.

And last but not least, to David Hansel for the patches for PA Tiny BASIC 1.0 and to Tom Lake for converting VTL-2 from a ROM to a .hex file.

## And the fine print...

The copyright for most of the various files are held by others and subject to various terms and conditions. I have tried to recognize those involved and have left any notices or attribution in place with the files used.

To the extent applicable, any original work is:

Copyright 2017 by James McClanahan and made available under the terms of The MIT License.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
