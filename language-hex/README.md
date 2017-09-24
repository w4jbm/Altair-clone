# Altair Languages

## General Instructions

These .hex files can be loaded on the Arduino-based Altair clone using the built in debugger using the approach described below.

* From power up, hit STOP and then RESET.
* Hold STOP+AUX1(UP) to enter the setup menu.
* From the terminal, enter "i" to enable input, "d" to enable debugging, and then 'x' to exit setup and dump into the debugger. You will see the PC (program counter), SP (stack pointer), registers, and such displayed, but you can ignore that.

Next you will need the .hex data. The main page where I found this was:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/

From there, you can find tinybasic-2.0.asm (the source), tinybasic-2.0.prn (the output from the assembler), and tinybasic-2.0.hex (the Intel Hex format output along with some other garbage).

The specific file you need is here:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/tinybasic-2.0.hex

This includes a symbols list at the start which you don't need. Jump down to the start of the hex data itself which looks like this:

 :100000003100203EFFC34206E3EFBEC368003E0D51
 :10001000F53A0008B7C36C06CD7103E5C32D03574D
  .
  .
  .
 :09076000077E236EE67F67F1E9D4
 :00000001FF

Highlight all of that and press Control-C to copy.

There are two leading spaces. Depending on the details, you may need to strip those out.

I used Tera Term and set the transmit delay to 10 milliseconds per character. (It also stripped the leading spaces for me. If that give anyone grief, I can pretty quickly strip them out with Notepad++ for you.)

You should still be in the monitor, so if you type 'H' you should see the message, "Reading HEX data...".

In Tera Term, I went to Edit and then Paste. It gave me a preview window (again, with the leading spaces stripped out for me) and I clicked okay to begin sending the hex file to the Altair clone.

You will see the results of each line of hex data as it loads. Something like this:

Reading HEX data...
0000: 31 00 20 3E FF C3 42 06 E3 EF BE C3 68 00 3E 0D
0010: F5 3A 00 08 B7 C3 6C 06 CD 71 03 E5 C3 2D 03 57
.
.
.
0750: 23 BE D2 50 07 23 D1 C3 3B 07 3E 7F 23 BE D2 5C
0760: 07 7E 23 6E E6 7F 67 F1 E9
Success.

At this point, you are dumped back into the debugger.

Now type '!' for a hard reset (the same as STOP+RESET from the front panel). This will, among other things, reset the PC (program counter) to 0. You will get the status dump again also.

Now type 'r' (it must be lower case) to begin running things (from the PC which we reset to 0 with the last command).

At this point, the terminal screen should clear (scrolling blank lines) and you should see the message:

TINY BASIC

OK
>_

You are at the prompt and can enter a simple program like

10 FOR A=1 TO 10
20 PRINT A
30 NEXT A

Backspace and such doesn't work as expected. (Probably set up for a teletype or such--something I might dig into at some point. The source code is well commented. But it does capture it and will just play it back when you do a LIST. Back in the day, this used to give me grief when I'd mess up. If you type something like "30 NN<backspace>EXT", it will look like "30 NEXT" on the screen when you type and when you LIST. But it is stored with the extra N and the <backspace> character. Sometimes if you get an error message on a line that looks perfectly fine, it is because of something like this and the only way to fix it is to reenter the line.)

You should be going in Palo Alto Tiny BASIC 2.0 at this point:

>10 FOR A=1 TO 10
>20 PRINT A
>30 NEXT A
>RUN
     1
     2
     3
     4
     5
     6
     7
     8
     9
    10

OK
>LIST
  10 FOR A=1 TO 10
  20 PRINT A
  30 NEXT A

OK
>_

All of that was fairly straight forward, so now it is back to getting the VTL bin file into hex format since I know I can load and run it. (Not that difficult to do, but it means switching over to the linux machine for a bit.)

Thanks,
Jim
