kernel make

target in Makefile

    _all:    ; [Makefile] begining
    
    PHONY += all     ; [Makefile] middle
    ifeq ($(KBUILD_EXTMOD),)
    _all: all
    else
    _all: modules
    endif
make auto combine the same target _all:, so all: will be the default target

all: vmlinux

all: modules

all: bzImage [arch/x86/Makefile]

    vmlinux: scripts/link-vmlinux.sh vmlinux_prereq $(vmlinux-deps) FORCE
    	+$(call if_changed,link-vmlinux)
    	
    modules: $(vmlinux-dirs) $(if $(KBUILD_BUILTIN),vmlinux) modules.builtin
    	$(Q)$(AWK) '!x[$$0]++' $(vmlinux-dirs:%=$(objtree)/%/modules.order) > $(objtree)/modules.order
    	@$(kecho) '  Building modules, stage 2.';
    	$(Q)$(MAKE) -f $(srctree)/scripts/Makefile.modpost
    	
    vmlinux_prereq: $(vmlinux-deps) FORCE, focus on $(vmlinux-deps)
    
    vmlinux-deps := $(KBUILD_LDS) $(KBUILD_VMLINUX_INIT) $(KBUILD_VMLINUX_MAIN) $(KBUILD_VMLINUX_LIBS)
    
    $(sort $(vmlinux-deps)): $(vmlinux-dirs) ;
    
    $(vmlinux-dirs): prepare scripts
    	$(Q)$(MAKE) $(build)=$@ need-builtin=1  ; equals to
    	make -f scripts/Makefile.build obj=$@   ; build in subdir
    
    

dot:  windows graphviz2.38\bin\gvedit.exe
digraph { 
node [color=lightblue2, style=filled];
  all->{vmlinux,modules}
  
  vmlinux->{rank=same vmlinux_prereq, "$(vmlinux-deps)"}
  vmlinux_prereq->"$(vmlinux-deps)"
  
  "$(vmlinux-deps)"->{rank=same "$(KBUILD_LDS)", "$(KBUILD_VMLINUX_INIT)", "$(KBUILD_VMLINUX_MAIN)", "$(KBUILD_VMLINUX_LIBS)"}
  
  
  "$(KBUILD_VMLINUX_INIT)"->{rank=same "$(head-y)", "$(init-y)"}
  
  "$(KBUILD_VMLINUX_MAIN)"->{rank=same "$(core-y)", "$(libs-y2)", "$(drivers-y)", "$(net-y)", "$(virt-y)"}
  
  "$(KBUILD_VMLINUX_LIBS)"->{rank=same "$(libs-y1)"}
}

the value of init-y will be the path init/ or file init/build-in.o, same to drivers-y net-y etc. in [Makefile]

the last dependency of vmlinux will link to vmlinux, $(call if_changed,link-vmlinux)

    cmd_link-vmlinux =                                                 \
    	$(CONFIG_SHELL) $< $(LD) $(LDFLAGS) $(LDFLAGS_vmlinux) ;    \    ; $< is scripts/link-vmlinux.sh
    	$(if $(ARCH_POSTLINK), $(MAKE) -f $(ARCH_POSTLINK) $@, true)

if_changed [scripts/Kbuild.include]
link-vmlinux will invoke cmd_link-vmlinux function [Makefile]
after $(call if_changed,link-vmlinux) will save all cmd to *.cmd file  ; in if_changed function
    

include/config/%.conf:[Makefile] will invoke silentoldconfig:[scripts/kconfig/Makefile], generate include/config/, include/generated/

make -n defconfig >make_defconfig_n.txt 2>&1
make -n >make_default.txt 2>&1  ; need make  to generate include/config, include/generated
 

gcc -Wp,-MD,scripts/basic/.fixdep.d  ; -MD write dependency to file