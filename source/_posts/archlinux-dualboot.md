---
title: archlinux与Windows的双系统启动 efi与mbr
date: 2019-08-13 14:40:23
tags:
 - ArchLinux
 - 双系统
categories:
 - ArchLinux
---

一般安装windows与linux的双系统是建议先安装windows，然后安装linux。先安装linux也是可以的，只是怕安装windows过程中，windows可能会自动覆盖一些分区。

mbr引导方式，grub2,添加双系统启动项

安装玩linux后，安装完grub并生成配置后
```
vim /boot/grub/grub.cfg 最下面加，XXXXXX是lsblk-fs的windows分区的uuid

if [ "${grub_platform}" == "pc" ]; then
	menuentry "Microsoft Windows Vista/7/8/8.1/10 BIOS/MBR" {
		insmod part_msdos
		insmod ntfs
		insmod ntldr     
		search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 XXXXXXXXXXXXXXXX
		ntldr /bootmgr
	}
fi
```

efi引导方式，grub2,添加双系统启动项

```
pacman -S grub efibootmgr dosfstools os-prober mtools

# esp分区挂载在/boot
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --recheck
os-prober  ;find Windows Boot Manager:Windows:efi
grub-mkconfig -o /boot/grub/grub.cfg
```

启动目录结构
```
/boot/
├── grub
├── EFI
│   ├── Boot
│   ├── GRUB        # 安装的efi grub引导
│   ├── MicroSoft   # microsoft/boot/bootmgrfw.efi，EFI下的才能被os-prober识别
```

efi启动机制：bios找到gpt磁盘，fat32分区，如果有EFI目录则为esp启动分区，列出EFI下的efi引导项，然后启动，比mbr的基于硬盘扇区字节的更直观方便