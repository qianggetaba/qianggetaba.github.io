kernel entry point instruction: arch/x86/boot/header.S

    .globl	_start  ; not at the very begining
arch/x86/boot/setup.ld for the linker scipt, will arrange the seciton postion

init/main.c#start_kernel  is the kernel entry point, the arch folder is bootstrap for arch related stuff.

make -n > make_output.txt 2>&1  ; output make commands
make V=1 > make_output_v1.txt 2>&1

after kernel compile, will generate file "*.o.cmd" contain the command that generate the *.o in each folder 

kernel makefile compile sequence:
- generate .config ; make menuconfig make defconfig
- determine the arch
- do arch related makefile, arch/x86/Makefile, output vmlinux
- compress vmlinux to bzImages
- module compile

Makefile ; top

$(CURDIR) ; pwd, current path

unexport  ; undefine variable

https://www.gnu.org/software/make/manual/html_node/Name-Index.html#Name-Index ;make function list

ifeq ("$(origin O)", "command line")  ; if O option in commandline

ifeq or ifneq MUST follow a endif

ifeq ($(skip-makefile),)  ; if skip-makefile not defined

ARCH		?= $(SUBARCH)  ; set the arch
SRCARCH := x86
hdr-arch  := $(SRCARCH)

include arch/$(SRCARCH)/Makefile  ; arch related makefile
export KBUILD_DEFCONFIG KBUILD_KCONFIG  ; KBUILD_DEFCONFIG = default config, 

-include include/config/auto.conf  ; start recursively

?= ; set variable only if the variable is not set

KCONFIG_CONFIG	?= .config
export KCONFIG_CONFIG   ; import all .config variable

$(shell ; put after string to shell command

uapi ; user space api, the uapi include files become the top level /usr/include/linux/ files

USERINCLUDE  ; uapi
LINUXINCLUDE ; all include dir

archprepare:  ; invoke arch/x86/Makefile#


vmlinux:  ; 重要target 

-include  ; no error when file not exist

make distclean  ; > mrproper > clean
make mrproper
make defconfig