# crossbootstrap
multiple architecture cross compilers build script

This script is able to generate a set of cross compilers, suitable for porting a linux distribution to a new architecture.
Is able to build cross compilers to arm, powerpc, mips, openrisc, csky, sparc64, ix86 etc architectures.

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

2) Cross build a minimal set of utilities, by means of the results of this script.
3) Using real hardware, system qemu or chrooted user qemu build the rest of packages

Above steps are already achieved, but are not stable enough for release.

The versions tested are:

 GCC  : 9.3.0
 LINUX: 5.4.46
 GLIBC: 2.30
 MUSL : 1.2.1
 BIN  : 2.33.1


Enjoy.

