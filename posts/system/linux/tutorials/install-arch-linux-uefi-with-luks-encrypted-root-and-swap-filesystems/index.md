# Install Arch Linux UEFI with LUKS encrypted root and swap filesystems


This method will ensure that your data is encrypted if your device is ever stolen. Nevertheless it might not be sufficient against such "CIA driven attacks" as the evil-maid or a coldboot attack.


## Before starting 
---
Download the archiso image from https://www.archlinux.org/ and its signature.

To ensure that the NSA / GCHQ, any other intelligence agency nor hacker has not altered your download, generate the checksum and compare it to the one provided by ARCH Linux or use gpg to ensure your archlinux-*.iso is exactly what the Arch developers intended. For example : 

```
$ gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
```



Create your USB bootable stick : 

- If you're on windows use Rufus (it never let me down) to create your bootable USB stick.
- If you're on OSX / BSD / GNU/Linux / Unix-Like, there you go: 

```
$ dd bs=4M if=archlinux-*.iso of=/dev/sdX
```


Select UEFI from your USB stick boot menu. If your USB stick fails to boot, ensure that Secure Boot is disabled in your UEFI configuration.

## WiFi
---
*Note : in mid-2020 the Arch Linux devs deprecrated the use of `wifi-menu`. If you need to connect to WiFi, you will have to add `iwd` in the pacstrap command and then enable it after we enter arch-chroot*

It is typically wiser to be hard wired to the network during installation. However, Arch supports WiFi-only installs :

```
# iwctl
```

Connect to your WiFi station :

```
# station <wlan device name> connect <wifi-station-name>
```

Enter your WiFi station's passphrase

## Partitions
---
Before you begin partitioning your disk you may want to write random data to the drive first with something like the following:

```
# dd if=/dev/urandom of=/dev/sdX
```

*Note : this can be a very time-consuming process, depending on the speed of your CPU and disk, as well as the size of the disk. If you don't write random data to the whole device, it may be possible for an adversary to deduce how much space is actually being used inside the encrypted partition. Personally I don't write random data to the disk as I just want to protect the disk from being accessed in case the computer is ever stolen.*

```
# cfdisk /dev/sdX
```


Create the partitions we need :

1. 100MB EFI partition (type : EFI)
2. 250MB Boot partition (type : Linux Filesystem)
3. 100% size partiton (type : Linux Filesystem)

Next, create the file systems for /boot/efi and /boot :

```
# mkfs.vfat -F32 /dev/sdX1
# mkfs.ext2 /dev/sdX2
```

## Encrypted partitions
---
```
# cryptsetup -c aes-xts-plain64 -y --use-random luksFormat /dev/sdX3
# cryptsetup luksOpen /dev/sdX3 luks
```
This will create a partition for rootfs, modify at your convenience if /home or other should be on separate partitions: 

```
# pvcreate /dev/mapper/luks
# vgcreate vg0 /dev/mapper/luks
# lvcreate --size 8G vg0 --name swap
# lvcreate -l +100%FREE vg0 --name root
```

Create filesystems on the encrypted partition : 
```
# mkfs.ext4 /dev/mapper/vg0-root
# mkswap /dev/mapper/vg0-swap
```

## Mounting
---
```
# mount /dev/mapper/vg0-root /mnt
# swapon /dev/mapper/vg0-swap

# mkdir /mnt/boot
# mount /dev/sdX2 /mnt/boot

# mkdir /mnt/boot/efi
# mount /dev/sdX1 /mnt/boot/efi
```

## Arch Linux boostrap 
---
First, edit the mirrorlist to optimize package download speeds. Copy one or two mirrors near your physical location to the top of the mirrorlist :

```
# nano /etc/pacman.d/mirrorlist
```

Everything after linux-firmware is optional but highly recommended to get networking, partition management, an editor and manual pages.

```
# pacstrap /mnt base base-devel grub efibootmgr linux linux-headers linux-firmware lvm2 dhcpcd iwd dialog wpa_supplicant nano man-pages
```


## Fstab
---
```
# genfstab -pU /mnt >> /mnt/etc/fstab
```

Make /tmp a ramdisk (add the following line to /mnt/etc/fstab): 

`tmpfs	/tmp	tmpfs	defaults,noatime,mode=1777	0	0`

Change relatime on all non-boot partitions to noatime (reduces wear if using an SSD)

## Chroot
---
```
# arch-chroot /mnt /bin/bash
```

Setup system clock (adapt to your location) : 

```
# ln -s /usr/share/zoneinfo/Europe/Helsinki /etc/localtime
# hwclock --systohc --utc
```

Set the hostname :

```
# echo MYHOSTNAME > /etc/hostname
```


Update locale : 

```
# echo LANG=en_US.UTF-8 >> /etc/locale.conf`
```
Set password for root : 
```
# passwd
```

Add a real user : 

```
# useradd --create-home USERNAME
```

set the password : 

```
# passwd USERNAME
```

Configure mkinitcpio with the modules needed for the initrd image :

```
# nano /etc/mkinitcpio.conf
```

Add 'ext4' to MODULES
Add 'encrypt' and 'lvm2' to HOOKS before filesystems

Regenerate initrd image : 

```
# mkinitcpio -p linux
```

Setup grub :

```
# grub-install
```

In /etc/default/grub edit the line 
`GRUB_CMDLINE_LINUX` to 
`GRUB_CMDLINE_LINUX="cryptdevice=/dev/sdX3:luks:allow-discards"`

then run:

```
# grub-mkconfig -o /boot/grub/grub.cfg
```

Exit the arch-chroot : 

```
exit
```

## Unmount
---
```
# umount -R /mnt
# swapoff -a
```

## Remove your USB stick
---
```
# reboot
```

You can now install all the rest of your favorite programs

## Make a good action ! 
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️


