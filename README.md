# VS_Faderbox_Editor
Electron based WebMIDI editor for VS_Faderbox

## Overview

Out of user request I've quickly cobbled together a WebMIDI based Editor for my Faderbox design as an alternative to the command line tool for generating sysex files (link below or [here](https://github.com/nativeVS/VS_SysEx_Generator)).
  
As should be easily evident, I've blatently copied the [Nakedboards MC-8](https://nakedboards.org/mc8.html) [settings](https://nakedboards.org/settings_mc-8.html?).
Once I finished the HTML file I decided to quickly wrap it in Electron to make it slightly more user friendly with prebuilt versions found below.

### Downloads

* [Windows Installer](https://github.com/nativeVS/VS_Faderbox_Editor/raw/main/dist/VS_Faderbox_Editor%20Setup%205.0.0.exe)
* [Mac DMG](https://github.com/nativeVS/VS_Faderbox_Editor/raw/main/dist/VS_Faderbox_Editor-5.0.0.dmg)
* [HTML Version](https://github.com/nativeVS/VS_Faderbox_Editor/blob/main/src/VS_Faderbox_Settings.html)
  
### Links

* [nakedboards](https://nakedboards.org/)
* [Faderbox](https://github.com/nativeVS/VS_Faderbox)
* [SysEx Generator](https://github.com/nativeVS/VS_SysEx_Generator)
* [ttapa's Control Surface Library](https://github.com/tttapa/Control-Surface)

## Build Notes

I used [electron-builder](https://www.electron.build/) and had no issues building across Windows and Mac.
This repo does not include the node_modules folder necessary to build.
The version number has been set to 5.0.0 to be in line with the numbering convention of the VS_Faderbox.

## SysEx Format
Copied from Faderbox page.
  
Channel and Fader Reassignment SysEx Format:
```
F0 [id] [id] [id] [dI] [s1] [s2] [cc] [f0] [f1] [f2] [f3] xx F7
```
With manufacturer ID (`[id] [id] [id]`), device id (`[dI]`) and sub ids #1 & #2 (`[s1] [s2]` which correspond to model number and version number for this device);
the Checksum (xx) includes the entire SysEx message (i.e. starting with the device ID) and should add up to `FF` with all preceding elements.
The Device ID is in the normal version fixed to 0x01.
  
Thus the SysEx string for the default assignment is as follows:
```
F0 00 21 57 01 02 05 01 01 0B 15 14 40 F7
```
To request the current Channel and Fader assignment the box responds to the following SysEx:
```
F0 [id] [id] [id] [dI] [s1] [s2] 0x11 xx F7
```
This will trigger the following response:
```
F0 [id] [id] [id] [dI] 0x00 [s2] [cc] [f0] [f1] [f2] [f3] xx F7
```

It will respond to the standard SysEx ID Requests:
```
F0 7E 01 06 01 F7 (device id 01) or
F0 7E 7F 06 01 F7 (ignore device id)
```
SysEx ID Request Reply:
```
F0 7E 01 06 02 [id] [id] [id] 01 01 00 02 00 00 00 05 F7
                              |   | |   | |---------- Software Version No: v5
                              |   | |---- Model Number: 00 02
                              |---- Family Code: 01 01
```

  
  
nativeVS - 2021
