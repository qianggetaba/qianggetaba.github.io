gdb -q --command=common.gdb

gdb vmware

    .vmx
    debugStub.listen.guest32 = "TRUE"
    debugStub.listen.guest64 = "TRUE"
    monitor.debugOnStartGuest32 = "TRUE"
    
    target remote :8832   ; gdb remote connect