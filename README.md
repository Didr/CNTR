[OCAP_Repo]: https://github.com/mistergoodson/OCAP
[Leaflet]: http://leafletjs.com/
[CarpeNoctem]: http://www.carpenoctem.co/
[Icons8]: https://icons8.com/
[AUTHORS]: https://github.com/CntoDev/CNTR/blob/master/AUTHORS
[LOGO]: https://github.com/CntoDev/CNTR/raw/develop/web/images/cntr-logo.png
[CNTR_Capture]: https://github.com/CntoDev/CNTR/blob/master/docs/cntr-format.md
[BugReport]: https://feedback.bistudio.com/T120253

![CNTR][LOGO]
# Carpe Noctem Tactical Recap (CNTR)

**CNTR** is a recording and playback system for ArmA 3. The system captures infantry and vehicle movement, weapons fire,
casualties and many other aspects of an armed operation and stores it for later playback in a 2D environment with
functionality similar to that of the ArmA 3 map. **CNTR** thus allows for both planning an operation as well as
reviewing team-performance and coordination post-operation.

**CNTR** started as a modification [**OCAP**][OCAP_Repo], modified to suit CNTO's specific needs, but eventually ended
up being a complete rewrite of it with a different capture and playback algorithms, and a completely new storage format.

## Features
* Server-side real-time capturing - no client modding required
* Extensively stress-tested - support for over 300 simultaneously active units on the battlefield!
* Capturing and reproduction of in-game placed map markers
* Streamable, crash-resistant capture format - capture file is usable as-is even after fatal crashes
* On-the-fly capturing and storage - capture file is ready to use on mission end
* Interactive web playback system
  * Event ticker with go-to-event and go-to-unit features
  * Interactive unit overview featuring individual unit status
  * Map preview - no capture file required

## Upcoming features (subject to change!)
* Automatic remote upload to another server
* Map drawing capabilities in web player similar to those in ArmA 3
* Collaborative web player sessions that allow users to plan and analyze operations outside of ArmA 3

## Installation guide (server-side only!)
* Please note that if the game server is not on the same server as the web player server, capture files will have to be
transferred manually together with the updated index file! Remote upload feature is in the works.*
* Download & extract the latest CNTR release
* Copy contents of `addons` folder into ArmA 3 root folder (i.e. `C:\Games\ArmA 3`)
* Copy contents of `userconfig` folder into ArmA 3 root folder
* Open `<ArmA 3 root folder>\userconfig\cntr\config.hpp` and modify the following line:
```
cntr_exportPath = "<EXPORT_PATH>";
```
to include the path where you placed the player. Make sure the path above is writable, otherwise the addon won't be able to copy the capture file and update the index!
* Copy `player` folder into web server's app folder (i.e. `C:\wamp64\www`) and name it as you wish (in this example, `cntr-player`)

## **IMPORTANT NOTE**
`Ended` mission handler is still bugged when using dedicated servers (see the related [bug report][BugReport]). One workaround is to manually stop the capture once the mission has been completed. 
The capture can be ended by the GM by executing the following command in the console:
```{ [] call cntr_fnc_stopCapture; } remoteExecCall ["call", 2]```

## Dev Setup Guide

* Install [Git](https://git-scm.com/)
* Clone CNTR repository to desired folder (in this example, `cntr`):
```
git clone https://github.com/CntoDev/CNTR.git cntr
```

### Addon
* Install **ArmA 3 Tools** from [Steam](http://store.steampowered.com/app/233800/ArmA_3_Tools/)
* Open **ArmA 3 Tools** and open the **Addon Builder**
* Under **Addon source directory** put the path to the addon source files, i.e. `cntr\addon\cntr`
* Under **Destination directory** put the path where the bundle should appear, such as directly to the ArmA 3 folder:
`<ArmA 3 root folder>\@cntr\addons\`
* Make sure the `Binarize` option is deselected!
* Click on **PACK** to create the addon bundle

### Extension (Windows - 64-bit only!)
* Install [mingw-w64](https://mingw-w64.org/doku.php) compiler suite (be sure to select `Architecture: x86_64` & `Threads: posix`)
* Open MinGW prompt
* Go to directory containing CNTR Exporter source files (`cntr/extension`)
* Compile DLL with following command
```
x86_64-w64-mingw32-g++ library.cpp -std=c++11 -shared -static-libgcc -static-libstdc++ -Wl,-Bstatic -lstdc++ -lwinpthread -o cntr_exporter_x64.dll
```

### Extension (Linux/Ubuntu - 32-bit only!)
* Open terminal
* Install g++ compiler
```
sudo apt-get install g++
```
or
```
yum install gcc-c++
```
* Go to directory containing CNTR Exporter source files (`cntr/extension`)
* Compile shared object file with following command
```
g++ -std=c++11 -fPIC -shared -m32 -o cntr_exporter.so library.cpp
```

### Marker creation
Inkscape is used to produce the `.svg` files.

* Use one of the previous source files (found in `web/images/mapMarkers` or `web/images/markers`) as a template
* Remove the current marker from the symbol-list (default key `ctrl + shift + Y`)
* Add the newly created marker to the symbol-list 
* Add `symbol` as the object ID to the symbol (default key `ctrl + shift + O`)
* Make sure only one symbol is found in the symbol list
* Using a text editor 
  * add the `viewport` attribute (found in the top of the document) to the `<symbol>` tag
  * Cleanup the `.svg` file by deleting unnecessary metadata, Inkscape settings and tags
  * For styling to be controled by CNTR, remove any styling added by Inkscape

### Web player
* Install latest version of [Node.js](https://nodejs.org/en/)
* Go to `web` directory of the repository
* Run `npm install`
* To bundle the app, run `npm run build`
* To start the watcher, run `npm run watch`
* To build for production (minified), run `npm run build:prod`

## CNTR capture format
CNTR uses its own human-readable capture format inspired by CSV and YAML formats. Please see the
[following document][CNTR_Capture] for detailed documentation.

## Credit list
* [Carpe Noctem's R&D team][CarpeNoctem]
* [Individuals highlighted in AUTHORS.md][AUTHORS]
* [Leaflet library][Leaflet]
* [UI icons by Icons8][Icons8]
* [Mistergoodson's original work on **OCAP**][OCAP_Repo]
