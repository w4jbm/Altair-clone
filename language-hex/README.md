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

## Palo Alto Tiny BASIC 2.0

The files associated with this originally came from:

http://www.autometer.de/unix4fun/z80pack/ftp/altair/

To build the All of that was fairly straight forward, so now it is back to getting the VTL bin file into hex format since I know I can load and run it. (Not that difficult to do, but it means switching over to the linux machine for a bit.)

Thanks,
Jim
