# UM2_switch
Transform your Ultimaker 2+ into dual extrusion system.

Time to upgrade my UM2 to dual extrusion printer. I got this switching mechanism idea already a year ago, but never took time to realize. Since the first tests seems to be promising, it's time to share this upgrade. It's working with a simple cam-plate, which is moving both nozzles at the same time. The cam-plate is bi-stable, by using the existing springs which also hold the teflon isolator in place.

Benefits of this modification, besides enabling 2 material prints:
- Simple modification on the existing hardware
- Switching is done above bed clips, so no loss of additional print space
- Modification works both on UM2 and UM2+ 

![UM2s dual nozzle lifting](https://github.com/KoosWelling/UM2_switch/blob/main/um2s_dual_nozzle_lifting.jpg)

## Hardware
Of course you will need a <a href="https://ultimaker.com/en/products/extrusion-upgrade-kit" target="blank">second extrusion train</a>.
Besides that, the good news is, the most important parts to be modified are already in your UM2. Basically you need to cut the bottom-plate in half, creating 2 levers. Modify them, so they could rotate. Bolt a bearing (W 627/3-2Z) on the front side of the top plate, used for the switch plate. 
The only part left is the cam-plate. 

In my case it's a laser cuted PolyStyreen plate. It surprisingly surveys already multiple days, none stop printing with 240 degrees nozzles & 70 degrees heated build plate.

Then on both sides of the printer, you need a 'switching-block'. Just a simple printed part should do the job. An other interesting working solution: use the UM side bearings as switching location. Then you only need a 'X' movement to do the switching action. But I leave this to your own imagination.

![UM2 parts modified](https://github.com/KoosWelling/UM2_switch/blob/main/um2s_part_modifications.png)
![UM2s assembly](https://github.com/KoosWelling/UM2_switch/blob/main/um2s_assembly.jpg)

## Firmware
The most important thing in the firmware, is tell your UM2(+) to use a second nozzle. There are different options:

* DIY: so download the right Marlin firmware from Github and change the number of extruders.
If you don't know how to do this, search the <a href="https://ultimaker.com/en/community/hardware" target="blank">UM</a> forum for more info.

* You might try this marlin version. 

* Or upload changes I made, yourselfs:
  * file: configuration.h
    - #define EXTRUDERS 2
  * file: UltiLCD2_menau_print.cpp:
    - #define HEATUP_POSITION_COMMAND "G1 F12000 X110 Y10"
    - Prime position = "G28 X115 Y10"
    - Pause position = "M601 X115 Y10 Z%i L%i"
These positions are changed, so when you do switching in the corner, you don't want to hit these positions while priming/pausing..

## Cura
Cura <3.4

Copy Cura definiions from the provided zipfile.
If you use newer version of Cura (most likely), you need to debug a bit.
If you have a working version, please upload/send!

Cura >3.4???

Within the *ultimaker2s_1st.def.json* extruder definition, it is/was possible to add an 'override':
* **"machine_extruder_start_code": { "default_value": "G0 F9000 X219 Y17.5\nG0 F1000 X223\nG0 F9000 X219" }**
This is basically the switching code, which will automatically implemented when slicing an object.
