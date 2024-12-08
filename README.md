# crossbootstrap
multiple architecture cross compilers build script

This script is able to generate a set of cross compilers, suitable for porting a linux distribution to a new architecture.
Is able to build cross compilers to arm, powerpc, mips, openrisc, csky, sparc64, ix86 etc architectures.

This is done on a orthogonal multiarch fashion. This allows the packages do not taint the root filesystem, by overwriting existing files. Also the "fossil" intermediate results are rearranged and kept, so it is possible to use them and build a new architecture root filesystem adding the /tmp/slackroot/kitchen/tools/bin to ${PATH} without installing the created packages. 

The only condition for this is to have the sources visible for the script. This can be achieved mouting ( or copying ) the slackware 15.0 sources over /var/cache/source/slackware/source and musl over /var/cache/source/legacy/l/musl. Placing this sources in /tmp/slackroot/dl is another option. 

This first step implies building a set of utilities ( gcc compiler, binutils, kernel headers, and libc ) in their cross flavor.
The script builds the packages in both glibc and muslc versions.
I strongly advise to test the packages over a chroot environement before to use them, specially outside slackware.
I've used it for bootstraping slackware to many new boards, it creates the packages as a tar archive, if you are not using slackware, or as a txz slackware package if you are.
The packages provided on the release are the slackware version, but you can untar the package over "/" and run /install/doinst.sh to install it on another distribution.

The sources, especially the gcc ada compiler are strongly entropic, so a minor issue like a newer or older version, or the particularities of your system may make your build break.
To start again you can remove the /tmp/slackroot working folder.

The layout is arranged in a fashion that does not taint the filesystem. The architecture dependent objects and includes are placed in architecture dependent folders.
Aditionally, the intermediate executables left behind after build are able to start to build packages. In orded to do that you must add /tmp/slackroot/kitchen/bin to your path

The sources can be placed from a slackware source tree on /var/cache/sources/slackware ( and /var/cache/sources/extra/l for musl ) or in  /tmp/slackroot/dl
The script gives information if the sources are not found.

The (sucessfull) new steps for porting are:

+ Cross build a minimal set of utilities, by means of the results of this script.
+ Using real hardware, system qemu or chrooted user qemu build the rest of packages

Above steps are already achieved, but are not stable enough for release.

The versions tested came from the Slackware 15.0 release:

 - GCC  : 11.2.0
 - LINUX: 5.15.19
 - GLIBC: 2.33
 - MUSL : 1.2.3
 - BIN  : 2.37

## script jobs

  #### slackcross
  The main builder. Builds the packages for a given architecture ( binutils, linux headers, glibc and/or musl, and cross gcc ) so you can build executables
  
  #### qemuscrapper
  Iterates the files made for the slackcross, and builds a statically linked qemu and a busybox for the architectures selected.
  
  #### slackjump
  Installs, lists and uninstalls the qemu helpers so you can run the foreing architecture in a trasparent way. It is possible to build a complete Slackware distribution this way.
  

Enjoy.

