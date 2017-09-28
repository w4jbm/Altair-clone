# Altair Languages

These files have been pulled together with help from and for use by various users of the Arduino-based Altair 8800 clone. Those interested in retro-computing are also often interested in the various languages used on early microcomputers. While most of this material has been available for some time (like since the 1970s in reality), it was not necessarily easy to load and use. The goal of this effort was to make them more widely available by making them easier to load and get started with.

In the good old days, most of these would have been loaded using a first-stage boot loader entered by hand to load a second-stage loader from paper tape. That second-stage loader would then have loaded the remainder of the tape and the actual program (or, in this case, language) into memory. Once that was done, the front panel would have been used to stop and reset the processor and then run the loaded program. If you want to fully relive that process, there are a few of these languages floating around in .tap (paper tape image) format along with information on how to go through the process of loading them.

That is one of those things that everyone should try at least once but can become comberson for many of us after the first dozen or so times...

General discussion or questions related to any of these languages for the Altair clone is typically found on the relevant group message board:

https://groups.google.com/forum/#!forum/altair-duino

Any additional contributions or ideas for languages to include is welcome. Several are "in progress" and will be placed here as they become available.

## General Instructions (Windows Oriented)

These .hex files can be loaded on the Arduino-based Altair clone using the built in debugger using the approach described below.

* From power up, hit STOP and then RESET.
* Hold STOP+AUX1(UP) to enter the setup menu.
* From the terminal, enter "i" to enable input, "d" to enable debugging, and then 'x' to exit setup and dump into the debugger. You will see the PC (program counter), SP (stack pointer), registers, and such displayed, but you can ignore that.
* Next you will need to load the .hex data which is contained in the files found in this directory to your clipboard.
* I used Tera Term and set the transmit delay to 2 milliseconds per character. *Remember that the delay is for communications with the debugger running on the Arduino, not a connection to the emulated Altair. Because of this, shorter delays may work.*
* You should still be in the monitor, so if you type 'H' you should see the message, "Reading HEX data...".
* In Tera Term, go Edit and then Paste. This will give you a preview window and the option to click okay and begin the transfer to the Altair clone.
* You will see the results of each line of hex data as it loads.
* Once the hex data has been loaded, you will be dumped back into the debugger.
* Now type '!' for a hard reset (the same as STOP+RESET from the front panel). This will, among other things, reset the PC (program counter) to 0. You will get the status dump again also.
* Now type 'r' (it must be lower case) to begin running things (from the PC which we reset to 0 with the last command).
* At this point, the terminal should should any startup messages associated with the language you are running as well as the prompt. There may also be questions such as memory size or terminal width to answer.

## General Instructions (Linux)

*I tend to use Linux, but have primarily used Windows in working with the Altair Clone to this point. Len provided the following guide to using Linux to load the .hex files.*

Here's how we're loading your .hex files here, running under Linux using the bash shell and using the Putty terminal emulator at 9600 baud (notes provided by my son). This will not work with "screen", as that software wants exclusive port access, which Putty doesn't, apparently:

* connect the Altairduino to your computer and wait for it to come up
* From power up, hit STOP and then RESET.
* connect with putty (don't forget to resize)
* Hold STOP+AUX1(UP) to enter the setup menu.
* From the terminal, enter "i" to enable input, "d" to enable debugging, and then 'x' to exit setup and dump into the debugger. You will see the PC (program counter), SP (stack pointer), registers, and such displayed, but you can ignore that.
* You should still be in the monitor, so if you type 'H' you should see the message, "Reading HEX data...".
* in terminal - get hex program in a file (for example tb_hex.hex)
* assuming serial is /dev/ttyACM0 run this - substitute the actual serial device and .hex filename for "/dev/ttyACM0" and "tb_hex.hex" shown here:
  * for i in \`cat tb_hex.hex\`; do echo -n "\*"; echo "$i" > /dev/ttyACM0;
  * sleep 0.1; done;echo
  * Note the backquote - \` - characters in the above, and watch the putty window as \*'s show up. You should get one per file line - and in the end - you should see "Success."
* on panel: hit STOP and then RESET
* then hit RUN and you should see it start up on putty

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

Copyright (c) 1977-1995 by Robert Swartz. All rights reserved.

## MINOL

MINOL first appeared in the article "MINOL--Tiny BASIC with Strings in 1.75K Bytes" by Erik T. Mueller in the April 1976 edition of Doctor Dobbs Journal.

You can follow the direction above and it seems to work fine. The .hex file initializes everything that seems to need to be initialized, including a "blank" program area. It would not hurt to type NEW, but it doesn't seem necessary.

The documentation and comments in the object code seem to indicated that $0000 is actually the "warm start" entry and that the "cold start" entry is at $02E8, but I get an error message when I start it that way instead of just starting execution at $0000.

You should end up at the MIDOL prompt which is a right bracket ("]")

Readable code is discussed a lot and MIDOL is the only language I've worked with that not only ignores spaces in the source code, but specifically prohibits them. The line input route

    ino:    RST     input
            MOV     B,A
            MOV     A,C
            CPI     0
            MOV     A,B
            JNZ     mid
            CPI     space
            JZ      ino             ; Do not accept space if outside quotes

Spaces are allowed in quoted text, but not in the source.

The command set is a bit different than the typical Tiny BASIC, so if you want to try something here is the code to count and print the number 1 to 10.

    10 LETX=1
    20 PRX
    30 LETX=X+1
    40 IFX<11;GOTO20
    99 END

The current .hex file is true to the original and assumes only 4K of RAM is available with less than half of that occupied by MIDOL. This value is stored as a little endian work at $0001 ($FF $0F) and can be modified. I modified some of the other languages to allow longer programs but, to be honest, I have a hard time imagining anyone writing a MIDOL program that will fill even the remainer of the 4K. (If you do, just patch the .hex file by adding a line just before the last record with something like :02000100FF7F7F.)

## Original Micro-soft BASIC 1.0

This is a copy of the original BASIC written for the Altair by Bill Gates and Paul Allen. Because it represents such an important part of microcomputer history, it is included here for informational and educational purposes only.

## Credit where credit is due...

Thanks to Mike Douglas at:

http://altairclone.com/

Mike has gathered an incredible amount of Altair material and I suspect that some of what floats around unattributed can often be traced back to his efforts. The Altair 8800 clone he offers is the "gold standard" among vintage computer clones.

Thanks to Chris Davis at:

https://www.altairduino.com/

His Arduino-based Altair 8800 kit is affordable and easy to build, making the fun of the Altair available to most who have an interest.

Thanks to Li-Chen Wang for the original Palo Alto Tiny Basic and the foundation of sharing that he created with his "COPYLEFT" and "ALL WRONGS RESERVED" quips.

I had a little trouble definatively determining who first wrote VTL, but thanks to Frank McCoy and Gary Shannon along with others who have built on it and kept it alive. I believe this version may have been originally recreated by Emmanuel Roche entering the original code from a hardcopy.

Thanks to Stephen A. Ness, author of XYBASIC, and to Robert Swartz, the current copyright holder who allowed it to be offered as an open source project. Also to Emmanuel Roche, Steven Hirsch, and Udo Munk for bringing it back to life.

Thanks to Erik Mueller, author of MINOL, and to Emmanuel Roche for reentering the original code from hardcopy.

Thanks to Bill Gates and Paul Allen for launching the language that a generation of programers grew up with and (for the most part) loved.

And last but not least, to David Hansel for the patches for PA Tiny BASIC 1.0, to Tom Lake for converting VTL-2 from a ROM to a .hex file, and Len & son for the Linux instructions.

## And the fine print...

The copyright for most of the various files are held by others and subject to various terms and conditions. I have tried to recognize those involved and have left any notices or attribution in place with the files used.

These are intended for personal use only. Any material will be removed at the request of the copyright holder or those holding other rights to it.

To the extent applicable, any original work herein is:

Copyright 2017 by James McClanahan and made available under the terms of The MIT License.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
