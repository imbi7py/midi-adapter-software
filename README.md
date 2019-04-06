[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# Disclaimer

Copyright 2019 PO-MA

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


# Overview

In this repository you will find the firmware for the pocket operator midi adapter. Hardware stuff can be found [here](https://github.com/PO-MIDI-Adapter/midi-adapter-hardware)

The code will only work on a **Teensy 3.6**

[![PO-MA](https://raw.githubusercontent.com/PO-MIDI-Adapter/midi-adapter-hardware/master/photos/title.jpg)](https://www.youtube.com/watch?v=iIQ18DAJAU0 "PO-MA")

## Main Features

### 1.Make any Pocket operator MIDI compatible
1. The adapter can act as a MIDI device and recveive MIDI signal via the micro USB port
2. The adapter can act as a USB host. It can power on and receive signal from one or more MIDI controllers directly
3. Specified MIDI signals are tranposed into GPIO output of the Teensy which we can used to "simulate" button press on the PO
3. MIDI Thru; all midi signal received are send to the MIDI out port
4. MIDI notes/channel/cc can be remaped to user's specs

### 2. Internal synth engine with dedicated sound output
1. Programmed using the Teensy Audio Library
2. 16 Midi controllable parameters
3. Runs on seperate MIDI channel

### 4. Volca FM velocity control
1. Transpose note velocity in to MIDI CC
## Flow Diagram

Here's the signal flow digram for the setup used in the previous video

![Flow](https://raw.githubusercontent.com/PO-MIDI-Adapter/midi-adapter-sofware/master/flow.png)


# Modes of operation

## Pocket Operator Control

  Midi channel can be changed by `po_midi_channel`
  
  All the buttons on the PO are triggerd by Midi notes. I didn't want to use MIDI CC because it doesn't tell us when the pads is   released.
  
  There are two modes of operation, mode one is for controller with more than 16 pads/keys and mode 2 for controller with 16 pads/keys +  at least 3 knob.  The modes can be switched by `po_cc_control`.

  1. When `po_cc_control` is 0, the adapter is in mode 1, this means all 23 buttons on the PO are assigned to sperate midi notes. See     below for more specification on midi assignemnt. **This is probably the mode you want to use and it is the mode used in the video**
  2. In Mode 2 (`po_cc_control` is 1), there is midi_cc_knob[9] which can switch the behavior of the midi keys/pads.
  
    1. midi_cc_knob_9 is 0  -> The midi pads/keys are assigned to the 1-16 buttons on the PO
    2. midi_cc_knob_9 is 1 -> The midi pads/keys are assigned to the play,write,sound,bpm,pattern,fx,special
    3. midi_cc_knob_9 is 2 -> The FX button is pressed, The midi pads/keys are assigned to 1-16 buttons
    4. midi_cc_knob_9 is 3 -> The special(top right) buttons is pressed. midi pads/keys are assigned to 1-16 buttons
    5. midi_cc_knob_9 is 4 -> The write buttons is pressed. midi pads/keys are assigned to 1-16 buttons. Switching to another mode from        this mode will pressed the write button again, turning the light off.
    
    In mode 2, sound pattern and volume are also controlled by MIDI CC
    1. midi_cc_knob_10 -> Sounds 1-16
    2. midi_cc_knob_11 -> Pattern 1-16
    3. midi_cc_knob_12 -> Volume 1-16
    
## Synth Engine Control
  The synth engine operates on its own MIDI channel `SYNTH_MIDI_CHANNEL`. The synth is largely inspired by [Teensy Polysynth](https://github.com/otem/teensypolysynth). There are 16 parameters that can be controlled via midi CC.
  
 1. midi_cc_knob_1 : vco1 shape
 2. midi_cc_knob_2 : vco2 shape
 3. midi_cc_knob_3 : Detune vco2
 4. midi_cc_knob_4 : vco 2 octave
 5. midi_cc_knob_5 : LFO Freq
 6. midi_cc_knob_6 : LFO Mod
 7. midi_cc_knob_7 : Filter Freq
 8. midi_cc_knob_8 : Filter Res
 9. midi_cc_knob_9 : Attack
 10. midi_cc_knob_10  : Decay					
 11. midi_cc_knob_11  : Sustain
 12. midi_cc_knob_12  : Released
 13. midi_cc_knob_13  : Delay time
 14. midi_cc_knob_14  : Delay feedback
 15. midi_cc_knob_15  : Mix vco1/vco2
 16. midi_cc_knob_16  : Volume
 
## Volca FM Velocity
 By default the adapter will read velocity from midi notes and tranpose them in to the correct MIDI cc signal and sent via MIDI-OUT
 The Volca FM channels are set by `volca_fm_midi_ch_1` and `volca_fm_midi_ch_2`. You can either set 1 or both channels
 
## Transpose Control 
 By default tranpose control are not send to the MIDI out, but you can change that by setting `disable_transport` to 0

# Modifying MIDI assignment 

## MIDI to PO Button
1. MIDI_NOTE_1 -> PO_BUTTON_9
2. MIDI_NOTE_2 -> PO_BUTTON_10
3. MIDI_NOTE_3 -> PO_BUTTON_11
4. MIDI_NOTE_4 -> PO_BUTTON_12
5. MIDI_NOTE_5 -> PO_BUTTON_13
6. MIDI_NOTE_6 -> PO_BUTTON_14
7. MIDI_NOTE_7 -> PO_BUTTON_15
8. MIDI_NOTE_8 -> PO_BUTTON_16
9. MIDI_NOTE_9 -> PO_BUTTON_1
10. MIDI_NOTE_10 -> PO_BUTTON_2
11. MIDI_NOTE_11 -> PO_BUTTON_3
12. MIDI_NOTE_12 -> PO_BUTTON_4
13. MIDI_NOTE_13 -> PO_BUTTON_5
14. MIDI_NOTE_14 -> PO_BUTTON_6
15. MIDI_NOTE_15 -> PO_BUTTON_7
16. MIDI_NOTE_16 -> PO_BUTTON_8
17. MIDI_NOTE_17 -> PO_BUTTON_SOUND
18. MIDI_NOTE_18 -> PO_BUTTON_PATTERN
19. MIDI_NOTE_19 -> PO_BUTT1. M
20. MIDI_NOTE_20 -> PO_BUTTON_SPECIAL
21. MIDI_NOTE_21 -> PO_BUTTON_FX
22. MIDI_NOTE_22 -> PO_BUTTON_PLAY
23. MIDI_NOTE_23 -> PO_BUTTON_WRITE

## MIDI notes 

1. MIDI_NOTE_1 : 60
2. MIDI_NOTE_2 : 61
3. MIDI_NOTE_3 : 62
4. MIDI_NOTE_4 : 63
5. MIDI_NOTE_5 : 64
6. MIDI_NOTE_6 : 65
7. MIDI_NOTE_7 : 66
8. MIDI_NOTE_8 : 67
9. MIDI_NOTE_9 : 68
10. MIDI_NOTE_10 : 69
11. MIDI_NOTE_11 : 70
12. MIDI_NOTE_12 : 71
13. MIDI_NOTE_13 : 72
14. MIDI_NOTE_14 : 73
15. MIDI_NOTE_15 : 74
16. MIDI_NOTE_16 : 75

17. MIDI_NOTE_17 : 76
18. MIDI_NOTE_18 : 77
19. MIDI_NOTE_19 : 78
20. MIDI_NOTE_20 : 79
21. MIDI_NOTE_21 : 80
22. MIDI_NOTE_22 : 81
23. MIDI_NOTE_23 : 82

## MIDI CC

1. MIDI_KNOB_1_CC : 1
2. MIDI_KNOB_2_CC : 2
3. MIDI_KNOB_3_CC : 3
4. MIDI_KNOB_4_CC : 4
5. MIDI_KNOB_5_CC : 5
6. MIDI_KNOB_6_CC : 6
7. MIDI_KNOB_7_CC : 7
8. MIDI_KNOB_8_CC : 8
9. MIDI_KNOB_9_CC : 9
10. MIDI_KNOB_10_CC : 10
11. MIDI_KNOB_11_CC : 11
12. MIDI_KNOB_12_CC : 12
13. MIDI_KNOB_13_CC : 13
14. MIDI_KNOB_14_CC : 14
15. MIDI_KNOB_15_CC : 15
16. MIDI_KNOB_16_CC : 16

# Uploading code
1. Download or clone the repo

2. You will need to first download and install [Arduino IDE](https://www.arduino.cc/en/main/software). Then download [Teensyduino](https://www.pjrc.com/teensy/td_download.html) and install on your computer in the same location where the Arduino IDE was installed.
You will also need to install the [sd config](https://github.com/bneedhamia/sdconfigfile) library 

3. Open Arduino IDE and open the .ino file.

4. Under Tools->Boards choose Teensy 3.6

5. Under Tools->USB Type choose Serial + MIDI16

6. With the Teensy plugged in,click upload to upload

The compiler might complain about an issue for conflicting library for the SD card, in this case you can delete the Arduino version found in `arduino/libraries/sd` 
