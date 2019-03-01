
#download ovmf efi firmware
vi /etc/yum.repos.d/kraxel.repo

    [qemu-firmware-jenkins]
    name=firmware for qemu, built by jenkins, fresh from git repos
    baseurl=https://www.kraxel.org/repos/jenkins/
    enabled=0
    gpgcheck=0


yum --enablerepo=qemu-firmware-jenkins install edk2.git-ovmf-x64

after install, check exist
ls /usr/share/edk2.git/ovmf-x64/OVMF_CODE-pure-efi.fd

vi /etc/libvirt/qemu.conf

    nvram = [
        "/usr/share/edk2.git/ovmf-x64/OVMF_CODE-pure-efi.fd:/usr/share/edk2.git/ovmf-x64/OVMF_VARS-pure-efi.fd"
    ]

systemctl restart libvirtd

two way start uefi

1>

    Create a new VM in virt-manager. When you get to the final page of the 'New VM' wizard, do the following:
    
    Click 'Customize before install', then select 'Finish'
    On the 'Overview' screen, Change the 'Firmware' field to select the 'UEFI x86_64' option.
    Click 'Begin Installation'
    The boot screen you'll see should use linuxefi commands to boot the installer, and you should be able to run efibootmgr inside that system, to verify that you're running an UEFI OS.
2>

    sudo virt-install --name f20-uefi \
       --ram 2048 --disk size=20 \
       --boot uefi \
       --location .
