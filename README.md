## ScottFree64/ScottFree128 - Reworking of ScottFree Revision 1.14b for the Commodore 64 and the Commodore 128

**Source**:  [https://github.com/mseelye/scottfree-commodore64](https://github.com/mseelye/scottfree-commodore64)  
**This Rework by**:  Mark Seelye <mseelye@yahoo.com>  
**Original**: ["ScottFree release 1.14b" for DOS, by Alan Cox](http://ifarchive.org/if-archive/scott-adams/interpreters/scottfree/scott.zip)  
**Copyright**:  
Scott Free, A Scott Adams game driver in C.  
Release 1.14b (PC), (c) 1993,1994,1995 Swansea University Computer Society.  
Port to ScottFree64 Release 2.1.0, (c) 2020 Mark Seelye  
Distributed under the GNU software license  
**License**:   GNU GPL, version 2 - see [GPL-2.0.md](https://bitbucket.org/mseelye/scottfree-64/src/master/GPL-2.0.md)  

## Credits and Thanks
* Original SCOTT.c/SCOTT.h source from "ScottFree release 1.14b" for DOS, by Alan Cox. From [ifarchive.org](http://ifarchive.org/indexes/if-archiveXscott-adamsXinterpretersXscottfree.html)  
* Mike Taylor's fixes for the **definition notes** from his [ScottKit](https://github.com/MikeTaylor/scottkit/tree/master/docs/notes) project.
* Jason Compton, who made a great Scott Adams-format game called "Ghost King" that inspired me to play around with Scott Adams Interpreters
* "Ghost King" is available in the /games directory, and included with ScottFree 64 on the .d64 and .d81 disk images.
* ALL the games in the /games directory belong to the authors who created them. I do not claim to have authored any of them, and I provide them here for the sake of convenience. I believe almost all of these came from [ifarchive.org](http://ifarchive.org/indexes/if-archiveXscott-adamsXinterpretersXscottfree.html)  
* More thanks for all the testing, advice and encouragement from Jason Compton, Robin Harbron, Adrian Gonzalez (dW), Per (MagerValp), Oliver V.(Six), Bob S.(Dokken), David (jbevren) and Sam.

## Goals for this Project
My goal here was to create a working version of ScottFree for the Commodore 64, with as few changes to the 1.14b SCOTT.C source as possible.
This wasn't an attempt to make this fast on the c64, or even run well. It was just an attempt to get it to compile and run.
I also used this to explore cc65 again, which was a lot of fun.

## Some Notes
* ~~**EVERYTHING LOADS VERY SLOWLY**, this is just a direct port, no real optimizations.~~  Update: if you use the game files with the suffix bdat, they will load MUCH faster. BDAT is "Binary DAT" see the [BDAT-README](games/BDAT-README.md) for more information.

* I removed some code that is not used. I also removed the -t TRS formatting option which forces display to 64 columns. This build of ScottFree uses the standard 40-column display.
* Thanks to some feedback from Jason, I added some code to **RESTART** the currently loaded game because reloading the whole thing is super painful.
* ~~ If you use the **-r** option it will also load your most recent save file when you restart the game by dying, winning, or quitting. ~~ Game prompts now.
* I hacked up the top/bottom display code to get the room desc appearing more consistently. 
This is not as effecient as the original Redraw logic, but without making many changes this was the easiest way to avoid display issues.
* I also have made yet-another-ncurses port that will compile and run on Windows(mingw/MSys2), MacOS, and linux (Tested on Ubuntu). I'll link that here once I finish that up.
* Based on feedback that black text on grey is "blah" I added the ability to use the F-keys to change the text, border, and background colors. **F1/F2** to inc/dec text color, **F3/F4** inc/dec border color, **F5/F6** inc/dec background color, F7 to restore to glorious monochrome "blah", **F8** to pick random colors for all.
* I've now completed some optimizations to replace some of the more bloated standard library calls with some custom ones.
* The optimizations have allowed this to easily run on the c128, it auto-detects 40/80 column mode as well.
* I've optimized memory usage on the Commodore 128, and can now store about 61k of "Messages" in Bank 1, making it able to run larger games, like "R (Pron, 'Arrr')".
* I eliminated the -r option, and now just have the game prompt to reload the last save or restart when you die/win/quit.

## Enough, how do I play?
You will need either a Commodore 64 emulator such as [Vice](https://vice-emu.sourceforge.io/), or some way to transfer the d64 or d81 files to a real 1541/1581 disk and play on your Commodore 64!  

* Download either the scottfree64.d64 and/or the scottfree64.d81 file from the dist directory.
* Open Vice, set the drive type to the appropriate type (1541/1581), and attach to the d64 or d81 respectively.
* Use `LOAD "*",8` or `LOAD "README64",8` and the `RUN`. This will load a BASIC stub readme I've created to help you understand the loading process, and how to pass arguments to the program.  
* The readme will explain that you need to first `LOAD "SCOTTFREE64",8`, THEN you must pass it any options you want to use, the game's file name, and optionally a save game file. 
* **Note:** ScottFree64 now supports a binary formatted DAT file called **BDAT**. These BDAT files are optimized to **load much more quickly** with ScottFree64 (starting with version 0.9.3). To use just use the bdat file instead of the dat file. (**ghostking.bdat** instead of ghostking.dat)  

Example:  

	LOAD "SCOTTFREE64",8
	LOADING  
	READY.
	RUN:REM -D GHOSTKING.DAT YORICK

In order to pass command line arguments to the program, binaries made in cc65 need to be run with the special `:REM` argument.  

The above statement is equivalent on a standard ScottFree target machine to  

`scottfree -d ghostking.dat yorick`

where `-d` is an optional switch, `ghostking.dat` is the playable game file to load, and `yorick` is the optional saved game.  

The options available in this build are similar to the original ScottFree 1.14b:  

* **-h**  Shows help similar to this README.
* **-y**  Use "you are" mode.
* **-s**  Use "scottlight" mode for light/darkness messages.
* **-i**  Use "I am" mode. (Default behavior, expected by most Adams-format games)
* **-d**  Debug. Shows detailed loading progress and data about the gamefile during the load.
* **-p**  Use old or "prehistoric" lamp behavior. 
* ~~**-r**  The game will restart from with the MOST RECENTLY SAVED game when you die, win, or quit. When enabled, you must reset your c64 and reload everything to restart the game from the beginning.~~ Game now prompts you to reload save or restart.

While in a game there are some other options available:  

* **save game**  Most, if not all, games allow you to save your game at any point by typing **save game** and hitting enter. It will prompt you for a name, enter a name to use and press enter.  To load a saved game when you start the program include the file name as the last argument when starting scottfree64. (see above)  
* **quit**  Most, if not all, games allow you to quit the game by typing **quit** and hitting enter. The game will prompt you to load the most recent save (if any) or restart the currently loaded game.  
* **F1 / F2** Change text color  
* **F3 / F4** Change border color  
* **F5 / F6** Change background color  
* **F7** Restore colors to starting colors (black text, grey background and border)  
* **F8** Picks random colors for text, background and border.  

## Commodore 128 version!
Follow the same steps above for the Commodore 64, except download the d64/d81 file called scottfree128.d64 or scottfree128.d81. 
Also, you will need to instead load the README128 file: `DLOAD "README128"`
To run in **80 column mode**, switch to 80 columns and load and run as described above while in 80 column mode. 

To build the Commodore 128 version use: `SYS=c128 make clean all`

## Games
Again, all of the games in the [games](games) directory belong to the original authors. Provided here for the sake of convenience.

#### Ghost King by Jason Compton
* **[Ghost King](https://ifdb.tads.org/viewgame?id=pv6hkqi34nzn1tdy)** by [Jason Compton](http://twitter.com/jpcwrites) "Your father is dead and you're sure your uncle is responsible. You tried to tell your mother so. Instead of believing you, she married him. Now you're going to uncover the truth and set things right..."

#### Scott Adams Classic Adventures (SACA)

* **adv01.dat**      [Adventureland](https://ifdb.tads.org/viewgame?id=dy4ok8sdlut6ddj7)
* **adv02.dat**      [Pirate Adventure (aka. Pirate's Cove)](https://ifdb.tads.org/viewgame?id=zya3mo3njj58hewi)
* **adv03.dat**      [Secret Mission(aka "Mission Impossible")](https://ifdb.tads.org/viewgame?id=89kxtet3vb9lzj87)
* **adv04.dat**      [Voodoo Castle](https://ifdb.tads.org/viewgame?id=ay2jy3sc3e6s9j4k)
* **adv05.dat**      [The Count](https://ifdb.tads.org/viewgame?id=89qmjvv4cb0z93t5)
* **adv06.dat**      [Strange Odyssey](https://ifdb.tads.org/viewgame?id=025dj4on6jr2c867)
* **adv07.dat**      [Mystery Fun House](https://ifdb.tads.org/viewgame?id=cj05ocxhay4dbrfs)
* **adv08.dat**      [Pyramid of Doom](https://ifdb.tads.org/viewgame?id=hew4c6rciycb6vog)
* **adv09.dat**      [Ghost Town](https://ifdb.tads.org/viewgame?id=rlxb5i0vjrnfr6x9)
* **adv10.dat**      [Savage Island, Part I](https://ifdb.tads.org/viewgame?id=wkaibkem4nxzo53y)
* **adv11.dat**      [Savage Island, Part II](https://ifdb.tads.org/viewgame?id=aqy6km542aq20jh4)
* **adv12.dat**      [The Golden Voyage](https://ifdb.tads.org/viewgame?id=4yo4je8dh53ug9qs)
#### Scott Adams later Adventures (plus a sample game)
* **adv13.dat**      [Sorcerer of Claymorgue Castle](https://ifdb.tads.org/viewgame?id=11tnb08k1jov4hyl)
* **adv14a.dat**     [Return to Pirate's Isle](https://ifdb.tads.org/viewgame?id=rp9eibu02f9vp2sv)
* **adv14b.dat**     [Buckaroo Banzai](https://ifdb.tads.org/viewgame?id=m85x5yr0zbopyuyb)
* **sampler1.dat**   [Mini Adventure Sampler](https://ifdb.tads.org/viewgame?id=7nkd8ib4xbeqr7pm)
#### Scott Adams later Adventures Marvel(TM) Adventures (Questprobe series)    
* **quest1.dat**     [The Hulk](https://ifdb.tads.org/viewgame?id=4blbm63qfki4kf2p)
* **quest2.dat**     [Spiderman](https://ifdb.tads.org/viewgame?id=ngi8ox3s9gfcand2)

These Scott Adams original text Adventure games are still copyrighted by Scott Adams and are not Public Domain. They may be freely downloaded and enjoyed though.
Please refer to the original shares at [ifarchive.org](http://ifarchive.org/indexes/if-archiveXscott-adamsXgamesXscottfree.html) Look for AdamsGames.zip  

#### The 11 Mysterious Adventures by Brian Howarth:  
(c) 1981,82,83. All games written by Brian Howarth. #5-7 co-written by Wherner Barnes, #8 and #11 co-written by Cliff J. Ogden.  

* **1_baton.dat**                     [The Golden Baton](https://ifdb.tads.org/viewgame?id=v148gq1vx7leo8al)
* **2_timemachine.dat**               [The Time Machine](https://ifdb.tads.org/viewgame?id=g7h92i8ucy0sfll2)
* **3_arrow1.dat**                    [Arrow of Death Part 1](https://ifdb.tads.org/viewgame?id=g25uklrs45gj7e02)
* **4_arrow2.dat**                    [Arrow of Death Part 2](https://ifdb.tads.org/viewgame?id=fy9klru0b5swgnsz)
* **5_pulsar7.dat**                   [Escape from Pulsar 7](https://ifdb.tads.org/viewgame?id=tbrvwzmvgmmnavhl)
* **6_circus.dat**                    [Circus](https://ifdb.tads.org/viewgame?id=bdnprzz9zomlge4b)
* **7_feasibility.dat**               [Feasibility Experiment](https://ifdb.tads.org/viewgame?id=up2ak731a6h2quna)
* **8_akyrz.dat**                     [The Wizard of Akyrz](https://ifdb.tads.org/viewgame?id=u1kmutsdwp8uys1h)
* **9_perseus.dat**                   [Perseus and Andromeda](https://ifdb.tads.org/viewgame?id=gti5j0nqvvqqvnzo)
* **A_tenlittleindians.dat**          [Ten Little Indians](https://ifdb.tads.org/viewgame?id=z7ettlqezn4mcnng)
* **B_waxworks.dat**                  [Waxworks](https://ifdb.tads.org/viewgame?id=lkt6sm3mgarb02bo)


All of the commercially published Scott Adams-format games available in TRS-80 .dat format are expected to work on this build of ScottFree. Some modern games which play at the boundaries of the Adams specification may fail on this build due to memory limitations or other quirks.

### DAT files / ifarchive.org  
**DAT** Is a format used by some of the Scott Adams game kits and players. 
You can find a lot of them on sites like:  
http://ifarchive.org/indexes/if-archiveXscott-adamsXgames.html

### BDAT files  
**BDAT** is a subset format of the DAT format that I made to optimize loading for ScottFree64.  
I've included a BDAT file for all the DAT files I had in the games directory.  To use just use **GAMENAME.BDAT** instead of GAMENAME.DAT!  
Please see the [BDAT-README](games/BDAT-README.md) for more information.  
(I'll soon publish tools to convert DATs to BDAT and BDATs to DATs.)  

## Building from source
I've used several tools to build the scottfree64 source, however the only tool required to build it is the cc65 toolset. **cc65** is a great cross development package for 65(C)02 systems. It has a macro assembler, a C compiler, a linker, and other tools. We are primarily interested in the compiler, assembler and the linker.  

The Makefile will complain if you don't have a required tool, and will warn you if you do not have an optional tool. With **cc65** installed, you should be able to update the Makefile with the location of your **cc65** installation by updating the **CC65_HOME** variable. You can then build the program with `make`.  With just **cc65* (Note: sed and cat are also used, but are not crucial.), and without c1541, petcat, and exomizer a build should look something like:

	$ make
	*** Compiling src/scottfree64.c
	/usr/local/cc65/bin/cc65 --static-locals -Ors --codesize 500 -T -g -t c64 src/scottfree64.c.tmp
	/usr/local/cc65/bin/ca65 src/scottfree64.c.s -o out/scottfree64.o
	*** Compilation complete
	*** Linking out/scottfree64.o
	/usr/local/cc65/bin/ld65  -o bin/scottfree64 -t c64 -m bin/scottfree64.map out/scottfree64.o c64.lib
	*** Note: exomizerX is not in PATH, cannot crunch bin/scottfree64,  please consider installing exomizerX from https://bitbucket.org/magli143/exomizer/wiki/Home
	*** Linking complete
	*** Note: c1541 is not in PATH, cannot build disk. please consider installing vice/c1541X from https://vice-emu.sourceforge.io/
	*** Note: c1541 is not in PATH, cannot build d81 disk. please consider installing vice/c1541X from https://vice-emu.sourceforge.io/
You can see the Makefile complains about the optional tools, but the file it produces in the /bin directory is a Commodore 64 executable. If you have **exomizer** installed it will use it to create a crunched Commodore 64 executable. **c1541** will put the readme, the executable, and some games on a d64 disk image, and a d81 disk image. Finally, if you have **petcat** installed it can also build the BASIC program used for the readme.

With all the tools installed a build looks like:  

	$ make
	*** Compiling src/scottfree64.c
	/usr/local/cc65/bin/cc65 --static-locals -Ors --codesize 500 -T -g -t c64 src/scottfree64.c.tmp
	/usr/local/cc65/bin/ca65 src/scottfree64.c.s -o out/scottfree64.o
	*** Compilation complete
	*** Linking out/scottfree64.o
	/usr/local/cc65/bin/ld65  -o bin/scottfree64 -t c64 -m bin/scottfree64.map out/scottfree64.o c64.lib
	"/usr/bin/exomizer" sfx sys -m 16384 -q -n -o bin/scottfree64.sfx bin/scottfree64
	*** Linking complete
	*** Tokenizing src/readme.c64basic with petcat
	"/usr/bin/petcat"  -ic -w2 -o src/readme.prg -- src/readme.c64basic.tmp
	*** Tokenization complete
	*** Building d64 disk...dist/scottfree64.d64
	formatting in unit 8 ...
	writing file `SRC/README.PRG' as `README' to unit 8
	writing file `BIN/SCOTTFREE64.SFX' as `SCOTTFREE64' to unit 8
	writing file `GAMES/GHOSTKING.DAT' as `GHOSTKING.DAT' to unit 8
	writing file `GAMES/SAMPLER1.DAT' as `SAMPLER1.DAT' to unit 8
	*** Disk Contents:
	0 "scottfree64     " bh 2a
	3     "readme64"          prg
	60    "scottfree64"       prg
	56    "ghostking.dat"     prg
	55    "sampler1.dat"      prg
	490 blocks free.
	*** Building d64 disk complete
	*** Building d81 disk...dist/scottfree64.d81
	*** Disk Contents:
	...(d81 listings omitted for brevity)

# Tools
### cc65 
**CC65** development tools are required to build this. CC65 binaries can be obtained for some platforms, but it can also be built from source.  
https://cc65.github.io/

### Vice / c1541 / petcat
**c1541** is a tool that comes packaged with the VICE emulator suite. Binaries for c1541 are available for most platfroms but it too can be built from source.  
**petcat** is also a tool that comes packaged with the VICE emulator suite. Binaries for petcat are available for most platfroms but it too can be built from source.  
https://sourceforge.net/projects/vice-emu/  
https://sourceforge.net/p/vice-emu/code/HEAD/tree/trunk/vice/src/Makefile.am (look for c1541-all and petcat-all target)  

### exomizer
**Exomizer** is a tool used to compresses files, it can generate a self-extracting program that is compatible with  cc65 arguments passing. Exomizer has a windows binary available, but can also be built from source. It is available from: (Look for the text, "Download it here.")  
https://bitbucket.org/magli143/exomizer/wiki/Home

### TMPx
**TMPx** (pronounced "T-M-P cross") is the multiplatform cross assembler version of Turbo Macro Pro, made by my friends and the c64 group "Style"!
TMPx was used previously to assemble the old BASIC shim "scottfree64-basic-loader.asm", however the shim grew and petcat was a better option. Check out TMPx anyway, it's a great cross assembler!
http://turbo.style64.org/

### make
I use GNU Make 4.3 in my Windows MSys2 environment, GNU Make 4.1 in my Ubuntu environment, and GNU Make 3.8.1 on my MacBook Pro.

### Various
The Makefile also makes use of various shell commands like **echo, cat, sed, rm,** and **mkdir**. Hopefully these are available in your build environment.

