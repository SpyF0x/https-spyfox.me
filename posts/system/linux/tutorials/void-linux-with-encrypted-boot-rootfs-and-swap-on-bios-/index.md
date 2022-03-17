# Void Linux w/ encrypted boot, rootfs and swap on BIOS


So today, we will see how to properly set up an uncomplicated Void Linux full disk encryption installation on legacy BIOS.


## First things first
---
In case you're wondering : 

> **1. _Why Void Linux ?_**

I've recently acquired an old laptop, so what's better for this old boi than an ultra lightweight *systemd free* GNU/Linux distribution ? 

> **_2. Why encrypted ?_**

It's a *(bad ?)* habit I got from work. 

Moreover, in those shady times where privacy has become a distant memory from the past, it's always good to properly encrypt your storage.


> **_3. Why encrypt boot and swap as well ?_**

Encrypting your boot sectors will keep you away from evil maid attacks, while encrypting your swap will ensure that no one will never recover any plaintext information from your computer. 

In my opinion, you should not be using any swap or at least an encrypted one but never ever use an unencrypted swap !


> **_4. You said BIOS ?_**

BIOS is awesome to do an uncomplicated real full disk encryption. Moreover, it is compatible with nearly any existing computer at the opposite of bloated next generation UEFI.

## Preparation
---
Obtain the latest Void Linux live ISO from the official [website](https://voidlinux.org/download/).

{{< figure src="/images/tutorials/void/encrypted-void1.png" alt="denial of service attack" >}} 


Unless you know what you are doing, choose glibc live xfce (<900 Mb) or glibc live base image (<600 Mb).

Of course, I strongly recommended you to validate the integrity and authenticity of any downloaded image before using it, to ensure it hasn’t been tampered. 

*Note: Void Linux devs provide you some instructions on how to do so in the [Void Handbook](https://docs.voidlinux.org/installation/index.html#verifying-images)*

Once everything is downloaded and verified, connect your USB stick and ask politely dd to do the work for you : 

```
# sudo dd bs=4M if=[IMAGE.ISO] of=/dev/sdX status=progress
```

Once the image has been written onto the USB stick, turn on the target computer (or reboot it) and boot from your USB stick.

If you don't know how to do that, please consider searching a bit online, as it strongly depends on your hardware.

## Installation  
---
Once Void Linux live is up and running, open up a bash shell and get comfy !
### Set your key map
Xfce : `$ setxkbmap [Country Code]`

base : `$ loadkeys [Country Code]`
### Set your Wi-Fi
Do the installation over Ethernet, but if somehow you really can't, here's how to set up Wi-Fi : 
```
# cp /etc/wpa_supplicant/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant-<wlan-interface>.conf

# wpa_passphrase <ssid> <passphrase> >> /etc/wpa_supplicant/wpa_supplicant-<wlan-interface>.conf

# sv restart dhcpcd
# ip link set up <interface>
```
##### Set up partitions (MBR)
First, tell your disk to use an MBR table :
```
# parted /dev/sdX mklabel msdos
```
Then create a single partition : primary partition, bootable flag, 100% size
```
# cfdisk /dev/sdX
```
### Format the partition (LUKS + EXT4)
```
# cryptset up luksFormat --type luks1 /dev/sdX1
```

*Note: By default, LUKS will be using `"--type luks2"` wich still has some bugs with GRUB 2.06*

Next, open it and create the filesystem. Old good EXT4 is my choice, but you can use any filesystem you want :

```
# cryptset up open /dev/sdX1 cryptroot
# mkfs.ext4 cryptroot -q 
```
*Note: `-q` means "quiet". It's just a personal preference* 
### Prepare the chroot :
```
# mount /dev/mapper/cryptroot /mnt
# mkdir /mnt/dev /mnt/proc /mnt/sys
# mount --rbind /dev /mnt/dev
# mount --rbind /proc /mnt/proc
# mount --rbind /sys /mnt/sys
```
### ~~Pacstrap~~ Bootstrap Void Linux !

```
# xbps-install -Sy -R https://repo-us.voidlinux.org/ -r /mnt base-system cryptset up grub nano
```
### Set up the new system

Chroot into your hard drive : 

```
# chroot /mnt /bin/bash
```
Then change the root password / default shell : 
```
# passwd
# chsh -s /bin/bash
```
Configure your timezone and your locales : 
```# ln -sf /usr/share/zoneinfo/Europe/Copenhagen /etc/localtime
# echo LANG=en_US.UTF-8 > /etc/locale.conf
# echo "en_US.UTF-8 UTF-8" >> /etc/default/libc-locales
# xbps-reconfigure -f glibc-locales
```
And finally change your hostname accordingly to your preferences : 
```
# echo <hostname> > /etc/hostname
```
### Initramfs encryption

When your computer will be booting up, Grub will ask you to enter a password. 

It will be used to decrypt the initramfs but once done, you'll need to enter a second password in order to unlock rootfs. 

To avoid that, we will be using a tiny hack consisting of making a key file generated with /dev/urandom : 

```
# dd bs=512 count=4 if=/dev/random of=/crypt.keyfile
# chmod 000 /crypt.keyfile
```
Create a variable where we will be storing our partition's UUID :
```
# uuid=$(sudo blkid -o value -s UUID /dev/sdX1)
dac4bfae-5172-463c-b9b5-6418e01c2f74
# cryptset up -v luksAddKey $uuid /crypt.keyfile
```
Apply proper permissions to /boot : 
```
# chmod -R g-rwx,o-rwx /boot
```
### Crypttab set up 
This will echo the UUID value of /dev/sdx1 to your crypttab : 
```
# echo $uuid >> /etc/crypttab
```
Once you have your UUID written in crypttab, just edit the file, so you have something similar to this at the end of the file : 

```
cryptroot UUID=dac4bfae-5172-463c-b9b5-6418e01c2f74 /crypt.keyfile luks
```

Finally, edit Dracut : 
```
# vim /etc/dracut.conf.d/10-crypt.conf
install_items+=" /crypt.keyfile /etc/crypttab "
```

*Note: Keep the **SAME** syntax with surrounding whitespaces !*
### GRUB set up

Redirect again your UUID : 
```
# echo $uuid >> /etc/default/grub
```
And edit Grub config file to include those lines : 
```
GRUB_CMDLINE_LINUX="cryptdevice=UUID=dac4bfae-5172-463c-b9b5-6418e01c2f74:cryptroot"
GRUB_ENABLE_CRYPTODISK=y
```
As well as "rd.auto=1" to GRUB_CMDLINE_LINUX_DEFAULT to force autoassembly of LUKS device :
```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=4 slub_debug=P page_poison=1 rd.auto=1"
```
Finally, the most critical step of this whole process, install Grub : 

```
# grub-install --recheck /dev/sdX
# grub-mkconfig -o /boot/grub/grub.cfg
# xbps-reconfigure -fa
```
*Note: if the previous grub-install command has failed, try to force BIOS targeting with this :*
```
# grub-install --target=i386-pc --no-floppy --recheck /dev/sdX
```

If you used some Wi-Fi during this installation, it might be useful to copy it over to the new install : 

```
# cp /etc/wpa_supplicant/wpa_supplicant.conf /mnt/etc/wpa_supplicant/wpa_supplicant.conf
```

Finally, reboot !

```
# exit
# umount -R /mnt
# reboot
```

Once reboot done, you should be prompted by GRUB for the password. Once the system is finished booting, log in.

You can now set up network services, users, and install additional packages...

## Endgame 
---
{{< figure src="/images/tutorials/void/encrypted-void2.jpg" alt="denial of service attack" >}} 
### Network service

Dhcpcd package is included in the base-system, so don't worry, you're not trapped !

```
# ln -s /etc/sv/dhcpcd /var/service/
```

This will automatically start the service. Once a service is linked, it will always start on boot and restart if it stops. 

If you need to keep an enabled service from starting automatically at boot, create a file named "down" in the service directory like this:

```
# touch /etc/sv/service_name/down
```
### User creation

Then we'll add a normal user (called "user" in this example) and add him to the wheel group :

```
# useradd -m -g wheel -s /bin/bash user
# passwd user
```

I also like to install the chrony daemon to keep clock in sync and socklog for logging and to enable ACPI events:

```
# xbps-install -Su
# xbps-install chrony socklog-void
# ln -s /etc/sv/chronyd /var/service/
# ln -s /etc/sv/socklog-unix /var/service/
# ln -s /etc/sv/nanoklogd /var/service/
# ln -s /etc/sv/acpid /var/service
```

You probably also won't need all the six running tty's, so you can remove some of them from `/var/service/`.


Then it's time to set up the relevant entries to the `/etc/hosts` file:

```
# vim /etc/hosts
127.0.0.1        localhost
::1              localhost
127.0.1.1        myhostname.localdomain myhostname
```
### Swap creation 

The last step is to create a swap file (if you want that). If you don't know how much should be your swap, don't worry !

Different people have a different opinion on ideal swap size. Even the major Linux distributions don’t have the same swap size guideline.

If you go by Red Hat’s [suggestion](https://www.redhat.com/en/blog/do-we-really-need-swap-modern-systems), they recommend a swap size of 20% of RAM for modern systems (i.e. 4 GB or higher RAM).

CentOS has a different [recommendation](https://docs.centos.org/5/html/Deployment_Guide-en-US/ch-swapspace.html) for the swap partition size. It suggests swap size to be:

    > Twice the size of RAM if RAM is less than 2 GB
    > Size of RAM + 2 GB if RAM size is more than 2 GB i.e. 5 GB of swap for 3 GB of RAM

Ubuntu has an entirely different perspective on the swap size as it takes hibernation into consideration. If you require hibernation, a swap of the size of RAM becomes necessary for Ubuntu.

Otherwise, it recommends:

    > If RAM is less than 1 GB, swap size should be at least the size of RAM and at most double the size of RAM

    > If RAM is more than 1 GB, swap size should be at least equal to the square root of the RAM size and at most double the size of RAM

    > If hibernation is used, swap size should be equal to the size of RAM plus the square root of the RAM size

Anyway, to create a swap file the size of your choosing simply use dd (here 4 GB): 
```
# dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
```
Set the right permissions:
```
# chmod 600 /swapfile
```
Format the file to swap:
```
# mkswap /swapfile
```
Activate the swap file:
```
# swapon /swapfile
```

Finally, edit the /etc/fstab configuration to add an entry for the swap file:
```
# vim /etc/fstab
/swapfile none swap defaults 0 0
```

You can check that the swapfile is up and running with:
```
# free
```

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
