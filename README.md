# Altair-clone
Various files related to the Altair and the Arduino-based Altair clone.

## QP/M on the clone...
The availability of a "search path" and more intuitive uses (and display) of user areas is important on hard drives. I installed QP/M on the hard disk image of the Altair. The only problem I had was with the location of the word used to store the default drive and user number. The default of $0008 does not work on the initial boot. Something else seems to overwrite this location as part of finalizing the boot process. (Once the system was booted and I set defaults, there didn't seem to be problems. But that didn't accomplish what I wanted.)

I tried several Page Zero locations without success, so eventually I looked towards the top of memory. I ended up using the location $FFFC which seems to work find. There is probably also some unused RAM towards the end of the CCP, but I haven't dug any deeper.
