# AllStarLink
AllStarLink on for the 3.x+ Linux Kernel

The files in this repository will compile successfully on a modern (3.x+) Linux Kernel (they have successfully been compiled on a Ubuntu 15.04 and runs fine).

Notes about using this (it is assumed you know how to compile software on linux, especially AllStarLink from scratch, and how to use patch for patching files.  This is provided *AS IS* without any warranty/guarantee that it will work for you.  This is a result of may hours searching the intenet and collecting various tidbits on AllStarLink.  It is published here in the hopes that it wil make someone else's life easier)

-You will need to ensure that all the build dependencies for asterisk are installed (apt-get build-dep asterisk)

-Download the files from the DAHDI directory, unpack the archive and use patch -p1 -i dahdi-allstar.patch to patch (note that you may be prompted for the files to patch, just look at the patch line and enter the full path and filename to patch.  If there are any rejects, and there may be one, check file with the extension .rej.  It should be fairly easy to add the line missing --- the dahdi-base.c file will have one patch reject, but it is only a single line that is missing and you can search for the line that follow it in the .rej file and add it to the dahdi-base.c file).  (Note: this version of the DAHDI driver with the applied patch are currently in use on my own Raspberry PI AllStarLink node and runs without issue).

-Once you have patched, compiled, and installed the DAHDI driver, use modprobe dahdi to make sure it load correctly (followed by an lsmod | grep dahdi)

-Download the files in trunk. Change to the trunk folder and execute make install_usbradio or make install_pciradio.  Asterisk will configure, compile and install.

-The biggest hurdle with getting this version of asterisk to compile on a newer version of linux was the res_crypto module.  The configure script was using a check that seems to be no longer supported in the newer versions of OpenSSL (it fails and results in res_crypto with an unresolved dependency).  I change the configure.ac file to use the checks from Asterisk 1.8's autoconf scripts, fixing the problem and results in res_crypto successfully compiling and working.

-If you use bootstrap.sh or autoreconf -iv to re-build the autoconfig environment, you will run across an error in the new ./configure script.  Autoconf puts in a check for SED (3 lines total) that you can safely comment out.

-You can skip directly to the asterisk directory and execute ./configure (instead of running make install_usbradio/install_pciradio in the parent directory).  If asterisk successfully configures (you will see ASCII art with instructions on how to install it), you can run a make install.

-One done, you will have to manually configure the node, since the download function from the AllStarLink portal probably won't work -- unless you are runnig a version of Linux that the auto configuration scripts support.

-The etc directory contains a script that you can run to have the nodelist downloaded on a periodic basis.  You will have to run before you first start AllStarLink, otherwise your node won't be able to connect to other AllStar nodes.

-The makefile directory contains three makefiles that you can use to replace their respesctive versions in trunk (note that the makefiles in trunk directory in this repository have already been replaced using these files)

-The chan directory contains the chan_alsaradio driver and sample config file -- this will allow you to use an un-modified USB soundcard with a serial port for PTT contol on a radio interface.

-Stacy (KG7QIN)
