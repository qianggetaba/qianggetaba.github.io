
kernel compile, [] is file path from kernel source root

make defconfig process

match target "%config:" [Makefile]

    %config: scripts_basic outputmakefile FORCE
    	$(Q)$(MAKE) $(build)=scripts/kconfig $@
FORCE is empty[Makefile]
outputmakefile not use

    scripts_basic:[Makefile]
    	$(Q)$(MAKE) $(build)=scripts/basic
    	$(Q)rm -f .tmp_quiet_recordmcount
$(build)

    include scripts/Kbuild.include [Makefile]
    build := -f $(srctree)/scripts/Makefile.build obj [scripts/Kbuild.include]
$(Q)$(MAKE) $(build)=scripts/basic equals to:

    $(Q)$(MAKE) -f $(srctree)/scripts/Makefile.build obj=scripts/basic  ; equals to
    make -f scripts/Makefile.build obj=scripts/basic
define include file, obj=scripts/basic, src=obj, 

    [scripts/Makefile.build]
    ; if src path has "/%", kbuild-dir=src, else kbuild-dir=srctree/src
    kbuild-dir := $(if $(filter /%,$(src)),$(src),$(srctree)/$(src))
    ; if file $(kbuild-dir)/Kbuild exist, then include kbuild, else include Makefile
    kbuild-file := $(if $(wildcard $(kbuild-dir)/Kbuild),$(kbuild-dir)/Kbuild,$(kbuild-dir)/Makefile)
    include $(kbuild-file)

so scripts_basic is include scripts/basic/Makefile, do other ...

%config now

    $(Q)$(MAKE) $(build)=scripts/kconfig $@    ; equals to
    make -f scripts/Makefile.build obj=scripts/kconfig defconfig
will include scripts/kconfig/Makefile, invoke target defconfig:

    defconfig: $(obj)/conf
    ifeq ($(KBUILD_DEFCONFIG),)
    	$< $(silent) --defconfig $(Kconfig)
    else
    ifneq ($(wildcard $(srctree)/arch/$(SRCARCH)/configs/$(KBUILD_DEFCONFIG)),)
    	@$(kecho) "*** Default configuration is based on '$(KBUILD_DEFCONFIG)'"
    	$(Q)$< $(silent) --defconfig=arch/$(SRCARCH)/configs/$(KBUILD_DEFCONFIG) $(Kconfig)
    else
    	@$(kecho) "*** Default configuration is based on target '$(KBUILD_DEFCONFIG)'"
    	$(Q)$(MAKE) -f $(srctree)/Makefile $(KBUILD_DEFCONFIG)
    endif
    endif
will build scripts/kconfig/conf.c program, the invoke the conf program

    KBUILD_DEFCONFIG defined after  [Makefile]
        include arch/$(SRCARCH)/Makefile
        export KBUILD_DEFCONFIG KBUILD_KCONFIG
    ifneq ($(wildcard $(srctree)/arch/$(SRCARCH)/configs/$(KBUILD_DEFCONFIG)),) ; if file is exist
    
    $(Q)$< $(silent) --defconfig=arch/$(SRCARCH)/configs/$(KBUILD_DEFCONFIG) $(Kconfig)  ; equals to  Kconfig := Kconfig [scripts/kconfig/Makefile]
    conf --defconfig=arch/x86/configs/x86_64_defconfig Kconfig

make menuconfig is the save process


