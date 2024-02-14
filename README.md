# Multi-tag Multi-anchor dwm1001 dev board official library usage

## Overview

This project utilizes the DWM1001-dev board from Decawave to measure the distance of a tag from four anchors.

The DWM1001 module is a Ultra Wideband (UWB) and Bluetooth hardware based on DecaWave's DW1000 IC and Nordic Semiconductor nrF52832 SoC. It allows to build a scalable Two-Way-Ranging (TWR) RTLS systems with up to thousands of tags.

# Requirements
* Works on (and tested on) Windows 11 Home x86_64 version 22H2
* SEGGER Embedded Studio for Arm V5.64 (64-bit)
* GNU ARM Embedded globally. Install the file: `gcc-arm-none-eabi-10.3-2021.10-win32.exe` from https://developer.arm.com/downloads/-/gnu-rm

# Hardware
* DW10001-DEV dev board with UWB chip and Nordic nrf52832_xxaa microcontroller

# Usage
1. Double-click `/DWM1001-Real-Time-Localization-System/dwm-simple/dwm-simple.emProject` to open in SEGGER IDE.
2. Click Build -> Rebuild {Project_Name}. You'll see output: "Build complete"
3. Make sure your board is connected via USB
4. To test with board run: Target -> Download {Project_Name}. You'll see "Download successful"
5. Follow the required code changes described in section `Software Changes Needed For Your Setup`, so set up 1 tags and 4 anchors (5 dev boards). Follow the `Download` step again for each burn of the software onto a board.
6. Connect all dev boards via USB to the computer at the same time, so we can communicate via serial RS232.
7. Open Windows Device Manager, expand section: `Ports COM & PLT` and see all devices that start with `JLINK CDC UART Port COM`.
8. Open Putty software for Windows in Serial communication. Set Baud rate to 115200 and COM string according to the device manager. For example, to connect to `JLINK CDC UART Port COM 14`, type-in `COM14` (create 5 instances of Putty, one for each dev board).
9. Press the reset button that is physically located on each dev board (there are 2 buttons, one of which is the reset button, the other is bluetooth switch). After each press of the button, you'll see output in Putty for that dev board:
```txt

 DWM1001 TWR Real Time Location System

 Copyright :  2016-2019 LEAPS and Decawave
 License   :  Please visit https://decawave.com/dwm1001_license
 Compiled  :  Mar 27 2019 03:37:38

 Help      :  ? or help

dwm> FW Major: 1
FW Minor: 3
FW Patch: 0
FW Res: 0
FW Var: 1
HW Version: deca002a
CFG Version: 00010700
Tag Set Stationary Detection: Disabled
Tag Set Measurement Mode: TWR
Tag Set Low Power Mode: Disabled
Tag Set Internal Location Engine: Enabled
Tag Set Encryption: Disabled
Tag Set LEDs: Enabled
Tag Set BLE: Enabled
Tag Set UWB Mode: Active
Tag Set FW Update: Disabled
Tag Get mode 0
Tag Get initiator 0
Tag Get bridge 0
Tag Get motion detection enabled 0
Tag Get measurement mode 0
Tag Get low power enabled 0
Tag Get internal location engine enabled 1
Tag Get encryption enabled 0
Tag Get LED enabled 1
Tag Get BLE enabled 1
Tag Get firmware update enabled 0
Tag Get UWB mode 2
PAN ID: 1
Set Position Quality Factor: 100
Set Position coordinates X: 10
Set Position coordinates Y: 10
Set Position coordinates Z: 0
Get Anchor Position [0, 0, 0]
Configurations Completed
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
number of anchor positions: 0   ->  [0,0,0,0]  [0]
```
10. Try to add a printf to the beginning of the function `dwm_user_start`, download again, press the reset button on the dev board, and see all of a sudden an additional print before: `DWM1001 TWR Real Time Location System`.

## Software Changes Needed For Your Setup
#### Initializing an Anchor

To initialize a device as an anchor:

1. Open the `main.c` file.
2. Set the `NodeType` to `Anchor`
```
  /* Set the node type to either Anchor (0) or Tag (1) */
  enum nodeType node_type = Anchor;

```
4. Configure at least one anchor as the initiator by setting `set_a_cfg.initiator = 1`.

#### Initializing a Tag

To initialize a device as a tag:

1. Open the `main.c` file.
2. Set the `NodeType` to `Tag`.
```
  /* Set the node type to either Anchor (0) or Tag (1) */
  enum nodeType node_type = Tag;

```

## Reproduce environment
1. Clone https://github.com/jonathanrjpereira/DWM1001-Real-Time-Localization-System and checkout to commit d3bf66444ca2bb5e85e8e801e66d4af0d2506d49

Place the entire repo of jonathanrjpereira as a subfolder of our project. Delete his .git folder.

That's used as a starting point to our project.

2. Download the entire `dwm` folder from here: https://github.com/iQuad427/iridia-dwm/tree/00531f45f9a206a1b582ad8740b680d359f89119/doc/DWM_Source_Docs_v11/DWM1001/Source_Code/DWM1001_on_board_package/DWM1001_on_board_package_R2.0/dwm

That dwm folder contains the official libraries (at `dwm/lib/libdwm.a` etc) according to the official documentation: `DWM1001 Firmware API Guide.pdf` https://www.qorvo.com/products/d/da007975 (search for "libdwm.a").

Files used for linking with the user applications:

1. dwm.h - header file - wrapper for all header files necessary for building of the user
application
2. libdwm.a - static library
3. extras.o, vectors.o, libtarget.a - eCos static library
4. target_s132_fw2.ld - linker script for firmware section 2

3. Place the contents of that dwm folder into the root directory of our project: 2 levels up from `/DWM1001-Real-Time-Localization-System/dwm-simple/dwm-simple.c`, since jonathanrjpereira's code assumes that the libraries and additional files are available 2 levels up from his project.

Specifically, the folders that need to be copied into our project from the `dwm` folder are: `include`, `lib`, `nordicsemi`, `recovery`, `utilities` (no need for `examples`).

4. Open the project in SEGGER by double-clicking `/DWM1001-Real-Time-Localization-System/dwm-simple/dwm-simple.emProject`

5. Tell the SEGGER project to use this Makefile `DWM1001-Real-Time-Localization-System/include/dwm.mak` based on the instructions available here: https://forum.segger.com/index.php/Thread/4726-SOLVED-Importing-ARMGCC-Makefile/?postID=17445#post17445

Select `Project {Project_Name}` in `Project Explorer` and then while it's selected go to the menu bar and click: Project -> Options...

Finally, go to Code -> External Build -> Build Command and change from nothing (default) to the full path to the `DWM1001-Real-Time-Localization-System/include/dwm.mak` file on your computer.

For example, on Chen's computer, change to:
```txt
"C:\Users\Chen1\Downloads\DWM1001-Real-Time-Localization-System\DWM1001-Real-Time-Localization-System-d3bf66444ca2bb5e85e8e801e66d4af0d2506d49\include\dwm.mak"
```
That ugly full path is only required for one build, and then SEGGER stores that `mak` configuration into the project after a single rebuild.

6. Still in the Options window, to to Code -> Build -> Toolchain Directory and change to the full path to your global GCC for ARM Embedded installation:
```text
C:/Program Files (x86)/GNU Arm Embedded Toolchain/10 2021.10/bin
````

7. You're done! Follow `Usage` steps to test if you've reproduced the environment correctly!
