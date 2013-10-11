====================
	TUTORIALS
====================
*) How to add new board.
	#) add a folder under boards:
		./boards/freescale/MYBOARD/
		
	#) configure, copy necessary parameters in that folder
	
	#) create config file under:
		./include/configs/MYBOARD.h
		
	#) add a line in:
		./boards.cfg
		

*) How to compile u-boot
	#) set up env parameters:
		$ export ARCH=arm
		$ export CROSS_COMPILE=../pathtocrosscompiler/...
		
	#) There are two ways to compile
		1) with one command:
			$ make MYBAORD
		2) with two commands (setting up configuration and then only using make)
			$ make MYBOARD_config
			$ make
			
*) Clean compiled stuff
	$ make distclean
	

*) Install freescales u-boot to device:
	$ sudo dd if=u-boot.imx of=/dev/XXX bs=512 seek=2
	$ sync
	
	
	
================
	Some info
================

-The u-boot provides information for linux kernel about which board this is. And then kernel
loads configuration according to that - there is no specific/different kernel for each board. 

-By default imx u-boot booting variables are set to load kernel from first FAT partition, 
where uImage should be resided, but this really depends on each devicd and u-boot version.

=====================
	Board specifics
=====================
-------------------------
SABRE tablet
-------------------------
*) Config name
	mx53smd
	
*) Specifics:
	#) (NOTE) U-boot freescale 2013.04 for SMD has some strange behaviour - it doesnt send its revision
	   number to linux kernel and thats why vpu encoder doesn't work. (solution is to use old u-boot or
	   fixed/updated version)
	#) By default 2013.04 u-boot SMD environment tries to boot from mmcpartition 2 and mount partition 3.
	   These values need to be adjusted if partitions are layed out differently. In example:
	   setenv mmcpart 1
	   setenv mmcroot /dev/mmcblk0p2 rw
	   

-------------------------
Baltic Embedded FIRST board
-------------------------
*) Config name
	BE_mx53_first
	
*) Specifics:
	#) Different RAM size.
	#) SD connected to different pin, thats why returncode is inverted. ( Thats why cant boot on SABRE from sdcard)
	#) Based on LOCO u-boot config, so kernel thinks this is LOCO board.




