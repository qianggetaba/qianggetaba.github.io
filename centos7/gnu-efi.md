
yum install gnu-efi gnu-efi-devel gnu-efi-utils

ldconfig


vi main.c  hello,world efi application

    #include <efi.h>
    #include <efilib.h>
     
    EFI_STATUS
    EFIAPI
    efi_main (EFI_HANDLE ImageHandle, EFI_SYSTEM_TABLE *SystemTable)
    {
      InitializeLib(ImageHandle, SystemTable);
      Print(L"Hello, world!\n");
      return EFI_SUCCESS;
    }
compile to object

    gcc main.c                             \
          -c                                 \
          -fno-stack-protector               \
          -fpic                              \
          -fshort-wchar                      \
          -mno-red-zone                      \
          -I /usr/include/efi                \
          -I /usr/include/efi/x86_64         \
          -DEFI_FUNCTION_WRAPPER             \
          -o main.o
link to elf

    ld main.o                          \
         /usr/lib64/gnuefi/crt0-efi-x86_64.o  \
         -nostdlib                      \
         -znocombreloc                  \
         -T /usr/lib64/gnuefi/elf_x86_64_efi.lds \
         -shared                        \
         -Bsymbolic                     \
         -L /usr/lib64/                 \
         -l:libgnuefi.a                 \
         -l:libefi.a                    \
         -o main.so
strip to binary

    objcopy -j .text                \
              -j .sdata               \
              -j .data                \
              -j .dynamic             \
              -j .dynsym              \
              -j .rel                 \
              -j .rela                \
              -j .reloc               \
              --target=efi-app-x86_64 \
              main.so                 \
              main.efi
qemu support efi test

    qemu-system-x86_64 -cpu qemu64 \
      -drive if=pflash,format=raw,unit=0,file=/usr/share/edk2.git/ovmf-x64/OVMF_CODE-pure-efi.fd,readonly=on \
      -drive if=pflash,format=raw,unit=1,file=/usr/share/edk2.git/ovmf-x64/OVMF_VARS-pure-efi.fd \
      -net none
prepare disk (gpt,fat32)

    dd if=/dev/zero of=uefi.img bs=512 count=93750   ;minimal 93750,don't little than 93750
    gdisk uefi.img   ;gpt
gdisk, note the ef00

     gdisk uefi.img
    GPT fdisk (gdisk) version 0.8.10
     
    Partition table scan:
      MBR: not present
      BSD: not present
      APM: not present
      GPT: not present
     
    Creating new GPT entries.
     
    Command (? for help): o
    This option deletes all partitions and creates a new protective MBR.
    Proceed? (Y/N): y
     
    Command (? for help): n
    Partition number (1-128, default 1): 1
    First sector (34-93716, default = 2048) or {+-}size{KMGTP}: 2048
    Last sector (2048-93716, default = 93716) or {+-}size{KMGTP}: 93716
    Current type is 'Linux filesystem'
    Hex code or GUID (L to show codes, Enter = 8300): ef00
    Changed type of partition to 'EFI System'
     
    Command (? for help): w
     
    Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
    PARTITIONS!!
     
    Do you want to proceed? (Y/N): y
    OK; writing new GUID partition table (GPT) to uefi.img.
    Warning: The kernel is still using the old partition table.
    The new table will be used at the next reboot.
    The operation has completed successfully.
mount and formant to write file

    losetup --offset 1048576 --sizelimit 46934528 /dev/loop0 uefi.img
    mkdosfs -F 32 /dev/loop0    ;with warning:Not enough clusters for a 32 bit FAT!
    mount /dev/loop0 /mnt
    cp main.efi /mnt/
    umount /mnt
    losetup -d /dev/loop0
qemu

    qemu-system-x86_64 -cpu qemu64 -bios /usr/share/edk2.git/ovmf-x64/OVMF-pure-efi.fd -drive file=uefi.img,if=ide
after qemu boot, see a new mapping with guid, type HD

    FS0:
    ls
    main.efi
 Reference:https://wiki.osdev.org/UEFI