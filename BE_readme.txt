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
		
	#) change CONFIG_MACH_TYPE in './include/configs/MYBOARD.h'. 
	   This is needed in case you have a custom kernel board configuration.
	   This number determines which kernel configs to load. Seems like there
	   is no standart naming possible in u-boot unlike it is in kernel. Thats
	   why we can simply invent a number which doesn't overlap and is defined
	   in kernel too:
	   
		#define CONFIG_MACH_TYPE	5555 
		
		
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
	

*) To update u-boot version from freescale the specific branch must be downloaded
   and merged into be-patches branch. Because mx53beboard is based on mx53loco,
   the configuration and board files must be checked if have changed and if have then
   they can be merged using some tool, e.g. :
   $ meld include/configs/mx53beboard.h include/configs/mx53loco.h

*) Install freescales u-boot to device:
	$ sudo dd if=u-boot.imx of=/dev/XXX bs=512 seek=2
	$ sync
	
	
*) Create patch for Yocto build - we are using the original freescale repository and applying
   our patches(boards) on to the freescale's source code. Because u-boot version and source code
   is being updated we need to update patches too.
   (The simplest solution is using diff and it does what it needs to be done. But in case there
   is needed 'standard-patch' then format-patch should be used and that involves rebasing which
   is cumbersome)
   
   #) Freescale's Yocto dylan branch uses u-boot version 2013.04, so if we need a patch for this
      to work with dylan we need to create patch against this version.
      ( patches-2013.04 ir original freescale branch, BE_v2013.04 is tag in BE be-patches branch,
      which is based on patches-2013.04, so includes changed from that version till this tag.)
      $ git diff patches-2013.04 BE_v2013.04 > patch
      
   #) The newest version will probably be HEAD/be-patches and patch can be applied similary, but
      in this case from HEAD against the branch used in Yocto u-boot recipe.

================
	Some info
================

-The u-boot provides information for linux kernel about which board this is. And then kernel
loads configuration according to that - there is no specific/different kernel for each board. 

-By default imx u-boot booting variables are set to load kernel from first FAT partition, 
where uImage should be resided, but this really depends on each device and u-boot version.


================================
	U-boot version differences
================================
*) 2013.04 -> 2013.10
	#) boards.cfg file syntax has changed slightly 
	#) licence header and config files updated for freescale boards

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
	   (This is fixed in 2013.10)
	#) By default 2013.04 u-boot SMD environment tries to boot from mmcpartition 2 and mount partition 3.
	   These values need to be adjusted if partitions are layed out differently. In example:
	   setenv mmcpart 1
	   setenv mmcroot /dev/mmcblk0p2 rw
	   (In 2013.10 boot works without modifications)
	   

-------------------------
Baltic Embedded BEBOARD
-------------------------
*) Config name
	mx53beboard
	
*) Specifics:
	#) Different RAM size.
	#) SD connected to different pin, thats why returncode is inverted. ( Thats why cant boot on SABRE from sdcard)
	#) Has custom MACH_TYPE, so it must exist in kernel too.




