# MPLAB PIC HID Flashing Tool

The *mphidflash* is a simple command-line tool for communicating with Microchip's USB HID-Bootloader and downloading new firmware. mphidflash supports Linux, Mac OS X (Leopard and later), and Windows.

Supports **Microchip USB HID Bootloader** for PIC16, PIC18, PIC24 distributed in the Microchip USB Library for Application, and Bootloader ports to the PIC32 such as for the UBW32 (Ultimate Bit Whacker 32). 

The default USB Vendor Identity *(VID) 0x04D8* and Product Identity *(PID) 0x003C*.

**NOTE:**  
The *mphidflash* does NOT support the Microchip PIC32 HID Bootloader
distributed in the Microchip MPLAB Harmony, and previously in source code
accompanying Application Note AN1388. Neither does *mphidflash* support the Bootloader in the Microchip PICDEM FS-USB demonstration application. The
communication protocol in these products is very different, and unfortunately the PIC32 HID Bootloader uses the same default USB Product ID as the supported HID Bootloader. If used in error either *mphidflash* or the development board may freeze or indicate an error.

The 'mphidflash' project is originally hosted at https://github.com/AdamLaurie/mphidflash (migrated from http://code.google.com/p/mphidflash/), but has been forked to a private VERO repository for internal use at https://github.com/VERO-Biotech/mphidflash.git 

# Building and Installing
VSCode was used with the WSL Extension utilizing the Linux Subsystem for Windows. This was configured to build the executable for windows on a Linux/Ubuntu environment as well as the standard native windows environment. This example will show you both ways to build the application.

## Linux
For Linux, you'll need the mingw-w64 toolchain (and associated
dependencies) and gcc natively installed.
```
sudo apt-get install mingw-w64
sudo apt-get install gcc
```
Build the application using the Ubuntu (WSL) sideloaded VM environment.

```
make -f Makefile.win
```

## Windows
For Windows, you can use mingw32 (cross) compiler which needs to be installed using *MSYS2* distribution and building platform for windows. You'll need the *mingw32* toolchain (and associated dependencies) installed.

https://www.msys2.org/

Build Instructions: 

1. Follow the instructions in the link above to install the mingw-w64 and mingw-32 GCC toolchains and their associated dependencies to the native path C:\msys64.

2. Add the path to the toolchains to the *Path* environment variable so that the OS can link them directly from powershell natively. You can look up how to do this in windows.

3. Open the *MSYS2 MSYS* terminal.

4. Update the package database and base packages.
	```
	pacman -Syu
	pacman -Su
	```

5. Install the *make*, *gcc* and *git* toolchains to get all the dependencies you need to build the application. The following command will give you a list of all the packages to install, select the packages and install the entire toolchains.
	```
	pacman -S --needed base-devel mingw-w64-x86_64-toolchain
	```
6. Open the *MSYS2 MinGW 32-bit* and *MSYS2 MinGW 64-bit* terminals and repeat the steps above to get all the toolchains installed. 

7. 	Open a powershell window and navigate to the repository and enter the following command. 
	```
	make -f Makefile.win
	```
This will create a .exe in the binaries sub-directory, which can be called
from the Windows commmand line (*mphidflash-1.8-win-32.exe*). You should copy this file somewhere on your executable path and rename it to 'mphidflash.exe'.

# Usage
To upload a new program to your PIC, it must be connected to your computer and set into bootloader mode. The tool can then be used with the following 
options:

```txt
-help			Display help screen (alternately: -?)
-write <file>	Upload given file to PIC 
-reset			Reset PIC
-noverify		Skip verification step
-erase			Erase PIC memory
-sign			Sign flash
-vendor <hex>	Use given USB vendor id instead of default id
-product <hex>	Use given USB product id instead of default id
```

## Example  
To upload the following *test.hex* to the PIC and to reset the PIC directly after the following command line can be used:
```
mphidflash -w test.hex -r
```
# Tips
For programming or erasing connect the development board directly to the PC or a powered hub, and consider using a short USB cable. The development board can rapidly change the amount of current drawn, and long cables can cause dips or spikes in the 5V connection. 

These operations are more likely to fail if there are unpowered hubs or extensions connecting the board to the PC.
