lsmod | grep fuse
modinfo --filename fuse

vmware-mount -p ./UEFI-disk1.vmdk  ; list partition
vmware-mount ./UEFI-disk1.vmdk /mnt
vmware-mount -d /mnt   ; umount

sudo vmware-mount -f ./UEFI-disk1.vmdk /mnt   ; the entire virtual disk to flat, sector
sudo vmware-mount -L   ; list
fdisk -lu /mnt/flat

dd if=/mnt/flat bs=512 skip=2048 count=1 | xxd  ;see sector