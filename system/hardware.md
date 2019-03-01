

smp  https://www.cheesecake.org/sac/smp.html
MP Floating Pointer Structure(oen of four place,signature "_MP_"), bios describe the hardware to os
- in the first kilobyte of the extended BIOS data area
- the last kilobyte of base memory
- the top of physical memory
- the BIOS read-only memory space between 0xe0000 and 0xfffff

    (gdb), find /w 0,0x200000, 0x5f504d5f  ;"_MP_"

MP Configuration Table(optional) (three parts: a header(signature "PCMP"), a base section, and an extended section)
- header section: (gdb)find /w 0,0x200000, 0x504d4350  ;"PCMP"
- base section: after header?
    - a set of entries
    - each entry 8bytes,processor entries 20bytes
    - 1st byte of entry is entry type
    - to find APIC ID of each processor, the address of the system's I/O APIC

local APIC, 0xfee00000 same place for each processor

APs start on real mode
- shutdown code by setting address 0:f to 0xa
- grab a page of memory for the AP's stack. You will also need space to store the `trampoline' code, i.e. the code the processor executes after waking up to switch to protected mode and jump to the kernel
- the start of the code must be at a page-aligned address
- 