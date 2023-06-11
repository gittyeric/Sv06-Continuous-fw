# SV06 Continuous Printing Mod!

Mostly just a branch of SV06's Marlin code, but supports continuous printing via part ramming!  This custom Marlin firmware is at the core of safe-ish automated continuous printing with a dependable, cheap printer! This fork uses TMC motor driver feedback to sense when a ram hits a stuck part and handle stopping and retrying.  Simply start a Y home (ex. `G28 Y R0`) starting from the max Y coordinate to trigger a "special" Y homing that will sense ramming a stuck part and pause / retry rams after further cooling.  After an hour of failed rams the print will be failed and block your continuous print queue until manual intervention.

WARNING!  Ramming your bare extruder head into a stuck part isn't a good idea.  Maybe consider a ram shield (link TODO) and bed slide (link TODO) to safely bump parts without damaging your extruder or jamming your printer with fallen parts.

DISCLAIMER! Nothing here is Sovol-approved, and I'm just a guy who's too lazy to clear his beds between prints.  Depending on your use you may deteriorate or even completely destroy your printer with this firmware over time and that's the risk you're accepting. If you don't like putting your machines at risk with slight hacks you can always buy a proper industrial printer or deal with the hassle of a belt printer!

### New Marlin Feature: Homing Ram

This firmware implements the "smart" homing ram, which does the following whe a `G28` homing command is issued for the Y axis but only when the bed is positioned all the way to the rear:

1. Ram forward in TMC "stall guard" mode, stopping if a stuck part is hit.
2. If a stuck part is hit, back up all the way, wait 15 minutes and try step 1 again, otherwise proceed as if a normal Y homing succeeded.  If this is the 4th failed attempt to ram, the printer will go into a kill state, preventing any continuous printing software from attempting more prints until manual intervention can safely clear and reset the printer.

## Installation

Included in this repo is the custom [Marlin Firmware build](continuous.bin) you can use to re-flash your SV06.  Take the SD card that came with your printer and remove all files from it (back them up first if you want, but they are available at Sovol's website).  Place [continuous.bin](continuous.bin) on the SD card, plug into the SV06, unplug the SV06's usb port (if connected), and restart the machine using the physical switch.  If the blank screen doesn't disappear, rename the file on the SD card to anything the printer hasn't seen before and repeat the process until the Sovol logo appears.  The display should now read `Sovol SC 1.x Ready` to indicate it's now a Sovol Continuous!  Running a Y home when the bed is all the way back will now trigger a collision-safe homing ram that will stop and retry if it detects a hard collision.

## In conjunction with G-Code

See [Continuous print farm DIY TODO](TODO) for how this firmware mod should be used with smart G-code for best effect.  If you use the suggested G-code scripting, you get the following smart pre and post print behavior:

### Pre-print

1. Wait for roughly 75% temps on bed and nozzle
2. Auto-home (if needed)
3. Place a dragging purge line across the X axis at the bottom of the bed, so a future rammed part scrapes the purge line with it!
4. Start print

### Post-print (TODO)

1. Wait for bed to cool to 35C (95F)
2. Wait anywhere from 15 to 60 minutes depending on first layer print surface area (larger prints stick to plate longer)
3. TODO-describe more

# (ORIGINAL README)

Remainder of this doc is the original SV06 branch README docs:

# Introduction

Sovol SV06 is a budget printer with compact functions. It adopts the movement structure of polished rod and bearings, thus, no wheels on the printer anymore!
It’s with an All Metal Hotend and a Planetary Gear Direct Drive Extruder, which supports printing with soft TPU and high-temperature filament, such as PETG, ABS, Carbon Fiber, etc. 
PEI Plate, 25 point Auto Leveling, 32bit Silent Mainboard, Dual Z axis, and Belt Tensioner are all pre-installed on the SV06. Welcome to check more on sovol3d.com

# Join Sovol Community

- Sovol Facebook page: https://www.facebook.com/sovol3d/
- Sovol Youtube Channel: https://www.youtube.com/c/Sovol/videos
- Sovol Offcial User Group: https://www.facebook.com/groups/sovol3d
- Sovol Forum website: https://forum.sovol3d.com/

# Related tutorials 

- User Manual. [Click here](https://drive.google.com/drive/folders/10uJUe-J0IutQSNI4IS-Tbwym4Ch8Yw6x)

# Learn more& How to Buy it

on Sovol Official Website: https://sovol3d.com/products/sovol-sv06-direct-drive-3d-printer

# Notice

This is the official source code for Sovol SV06. The damage caused by modifying firmware also using the third party firmware will lose the 1 year warranty. If you need support, it’s recommended to reflash the stock firmware before contacting sovol.

Sovol doesn’t provide tech help for help users to modify source code, but if you need us to add more functions, you are welcome to send us your suggestions via Facebook Messenger or email 
info@sovol3d.com





