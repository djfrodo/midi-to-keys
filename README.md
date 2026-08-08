# midi-to-keys

This project was started to create a simple macro pad for my mother who has macular degeneration - the full blog post can be found here:

https://www.headcycle.com/h/visuallyimpaired/comments/5e6029572cd4/a-small-tactile-web-assistant-for-the-visually

I wanted a small footprint with soft pads that was easy to program, could be color coded, and control any application (web browser, screen reader, etc).

I tried this in Windows using CoyoteMidi (see link above) and it turned out really well.

I later tried VIA and VIAL macropads and on Windows it was a disaster.

After which I wondered if this could be done in linux and...I never thought I would want a macro pad, but I now use it all the time.

The midi-to-keys scripts connect 16 pad midi controllers to commands that control applications in linux.

There are three scripts - two for X11, and one for Wayland (MacOs version coming soon). The only difference between the x11 scripts are the name of the controller in the code - one is for the SMC-PAD, and one is for the Jamjum.

Currently these scripts have been tested on two controllers - the M-Vave SMC-PAD Pocket, and the JamJum Pocket (or under another name the JP-MINI). Both also have different names depending on re/seller, but basically these are the basic 16 pad midi controllers.

## X11

For older hardware that still uses X11 xdotool needs to be installed: https://github.com/jordansissel/xdotool, https://superuser.com/questions/1170136/translating-midi-input-into-computer-keystrokes-on-linux
```
apt-get install xdotool
```
PLEASE NOTE: xdotool ONLY works in X11, not Wayland!!!

## Wayland

For Wayland alternatives for xdotool see: https://www.reddit.com/r/linuxquestions/comments/u5mxzi/xdotool_alternative_for_wayland/ or use your google fu.

## In Use

To list the midi controllers connected in X11:
```
aseqdump -l
```
Output will look something like this:
```
Port    Client name                      Port name
  0:0    System                           Timer
  0:1    System                           Announce
 14:0    Midi Through                     Midi Through Port-0
 16:0    SINCO                            SINCO SMC-PAD Pocket-Private
 16:1    SINCO                            SINCO SMC-PAD Pocket-Master
 16:2    SINCO                            SINCO 
```
To see what midi keys are being pressed from the SMC-Pad Pocket:
```
aseqdump -p "SINCO"
```
After hitting all 16 pads (in order left to right) the output (currently) will be:
```
Waiting for data. Press Ctrl+C to end.
Source  Event                  Ch  Data
 16:0   Note on                 9, note 48, velocity 127
 16:0   Note off                9, note 48, velocity 64
 16:0   Note on                 9, note 49, velocity 127
 16:0   Note off                9, note 49, velocity 64
 16:0   Note on                 9, note 50, velocity 127
 16:0   Note off                9, note 50, velocity 64
 16:0   Note on                 9, note 51, velocity 127
 16:0   Note off                9, note 51, velocity 64
 16:0   Note on                 9, note 44, velocity 127
 16:0   Note off                9, note 44, velocity 64
 16:0   Note on                 9, note 45, velocity 127
 16:0   Note off                9, note 45, velocity 64
 16:0   Note on                 9, note 46, velocity 127
 16:0   Note off                9, note 46, velocity 64
 16:0   Note on                 9, note 47, velocity 127
 16:0   Note off                9, note 47, velocity 64
 16:0   Note on                 9, note 40, velocity 127
 16:0   Note off                9, note 40, velocity 64
 16:0   Note on                 9, note 41, velocity 127
 16:0   Note off                9, note 41, velocity 64
 16:0   Note on                 9, note 42, velocity 127
 16:0   Note off                9, note 42, velocity 64
 16:0   Note on                 9, note 43, velocity 127
 16:0   Note off                9, note 43, velocity 64
 16:0   Note on                 9, note 36, velocity 127
 16:0   Note off                9, note 36, velocity 64
 16:0   Note on                 9, note 37, velocity 127
 16:0   Note off                9, note 37, velocity 64
 16:0   Note on                 9, note 38, velocity 127
 16:0   Note off                9, note 38, velocity 64
 16:0   Note on                 9, note 39, velocity 127
 16:0   Note off                9, note 39, velocity 64
 ```
To change what each key does edit the midi-to-keys or midi-to-keys-wayland files (see source).

Out of the box the SMC-PAD and the Jamjum have the same default midi notes, but the name in the script "SINCO" is for the SMC-PAD, and "JP-Mini" for the Jamjum. Obviously this needs to be changed depending on which you're using.

To automatically launch a background process on boot use the following in Startup Applications in Ubuntu:

Name: Midi to Keys Startup Script

Comments: < your text here >

Command
```
/home/< username >/midi-to-keys/midi-to-keys
```
or 
```
/home/< username >/midi-to-keys/midi-to-keys-wayland
```
It is suggested that you put this repository in your home directory. 

You'll also have to do the following to make the script you use executable:
```
chmod +x <script name>
```
These SMC-PAD scripts don't have the .sh extention, the Jamjum does...and it doesn't matter, if you really want the extention, add the changes accordingly.

If you've turned either of the pads off and then back on the script won't work. The terminal command to relaunch the script(s) in a background process is:
```
nohup /home/< username >/midi-to-keys/< midi-to-keys script name > /dev/null 2>&1 &
```
This command can also be used as the startup script command. In fact it's probably preferable to the simple startup command above, but either will work.

The Jamjum has 3 pad banks which are accessible via the "Pad Bank" key, while the SMC-PAD Pocket needs to be rebooted to change its pad bank. With that said the SMC-PAD Pocket has a much cleaner look due to the omission of the branding and top row of buttons.

In terms of software to change the colors of the pads the Jamjum is much better, but both are kind of janky and you'll need Windows (SMC-PAD) or Windows, MacOS, Android, or iOs (Jamjum). I didn't change the default notes of the midi keys on either, but the SMC-PAD definitely needs an increase in sensitivity.

Good luck!
