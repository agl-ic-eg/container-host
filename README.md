# Container host repo

This image is based on yocto thud release.  

## Building the BSP for M3 Starter Kit

Currently using R-CarGen3 BSP 3.21.

### Clone build tree.

Move to build directry

	export WORK=`pwd`  
	repo init -u https://github.com/agl-ic-eg/container-host.git  
	repo sync  

### Download proprietary driver modules to $WORK/proprietary folder.

You should see the following files:

	$ ls -1 $WORK/proprietary/*.zip  
	R-Car_Gen3_Series_Evaluation_Software_Package_for_Linux-weston5-20190802.zip  
	R-Car_Gen3_Series_Evaluation_Software_Package_of_Linux_Drivers-weston5-20190802.zip  


### Populate meta-renesas with proprietary software packages.

	export PKGS_DIR=$WORK/proprietary  
	cd $WORK/meta-renesas  
	sh meta-rcar-gen3/docs/sample/copyscript/copy_evaproprietary_softwares.sh -f $PKGS_DIR  
	unset PKGS_DIR  


### Setup build environment

	cd $WORK  
	source poky/oe-init-build-env  

### Prepare default configuration files.  

	cp $WORK/meta-renesas/meta-rcar-gen3/docs/sample/conf/m3ulcb/poky-gcc/mmp/*.conf ./conf/  
	cd $WORK/build  
	cp conf/local-wayland.conf conf/local.conf  

### Edit bblayers.conf with layer requirements:

	  ${TOPDIR}/../meta-openembedded/meta-networking \  
	  ${TOPDIR}/../meta-openembedded/meta-python \  
	  ${TOPDIR}/../meta-openembedded/meta-filesystems \  
	  ${TOPDIR}/../meta-virtualization \  
	  ${TOPDIR}/../meta-container-host \  
	  ${TOPDIR}/../meta-container-host/boardspecific/meta-container-renesas \  


### Edit local.conf with evaluation packages requirements:

	DISTRO_FEATURES_append = " use_eva_pkg"  
	
	DISTRO_FEATURES_append = " virtualization"  
	IMAGE_INSTALL_append = " lxc nano "  
	#INHERIT += "rm_work"  

### Start the build

	bitbake core-image-minimal  



## Building the BSP for QEMU

TBD


