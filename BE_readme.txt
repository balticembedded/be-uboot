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

By default imx u-boot booting variables are set to load kernel from first FAT partition, 
where uImage should be resided.
