# DIY - Turn an old PC into a powerful Firewall using OPNsense


Today, I will show you how to build a powerful open source (FOSS) router secured with a dedicated firewall and its own IDS (Suricata) at no cost. 

Go get your old laptop or PC get follow this DIY !


## First things first
---
When I talk to someone about cybersecurity, I often hear the price argument coming up. "Yeah, if you want a real level of security, you need dedicated equipment, and it costs a lot of money, etc."

It is perfectly true that dedicated hardware is not cheap for the casual Internet user (if you don't trust me, search for firewall prices into internet). The main reason for this is, is that the main client for this kind of equipment is not the everyday internet user but companies. As their wallet is still bigger than bob or alice, the price will always be higher. 

*Wait a minute, we have to be rich to be safe online ?*

- No ! 

Here's a freshly baked DIY to recycle any old laptop or PC into a powerful dedicated equipement to secure you and your family or your small company !

## Hardware
---
A router is basically a computer with at least 2 network interfaces. It can be a WiFi interface and an Ethernet one, it doesn't matter. 

Our DIY router will be interfacing your current router (ISP router) and your local area network (LAN). 

{{< figure src="/images/tutorials/opnsense/diy-opnsense2.jpg" alt="opnsense/diy-opnsense" >}} 

As there would be so many possible architectures, we will consider our super DIY router to be an old laptop with a WiFi interface (wlan0) with an Ethernet card (re0).
 
And because it only costs five bucks, we'll just add an Ethernet to USB dongle (usb0) to make a second ethernet interface. 

## Network plan 
---
{{< figure src="/images/tutorials/opnsense/diy-opnsense.png" alt="opnsense/diy-opnsense" >}} 
### RED ZONE 🔥
---
We will consider re0 to be facing the internet because we don't trust ISP router, and also it's the fastest interface.
### ORANGE ZONE 🍊
---
As it's WiFi, anyone in your WiFi range could get into the network (see this [tutorial](/tutorials/wifi/wifi-crack/wifi-crack-your-neighbors-wifi-wep-wpa-wpa2-copy/)) but it's not as wide as the entire internet.
### GREEN ZONE 🫒
---
The old good wired LAN is considered as the safest zone because an intruder will have to physically connect to our usb0 interface.

*Note: I've added on the diagram a switch because you can get good switches for cheap (5-8 ports : from 10 to 20 bucks) but it's only optional*

## Prepare OPNsense
---##### A little bit of background 

OPNsense is an open source firewall and routing platform that is easy to use and implement. 

OPNsense has most of the features expected of an expensive commercial firewall. It offers a rich feature set with the benefits of open and verifiable sources.

{{< figure src="/images/tutorials/opnsense/diy-opnsense2.png" alt="opnsense/diy-opnsense" >}} 

It is based on HardenedBSD and started as a fork (development from ...) of pfSense® and m0n0wall in 2014, with its first official release in January 2015. 

The project has evolved very quickly while retaining the familiar aspects of m0n0wall and pfSense. The development of the project is based on security and code quality.
### Download 

Download the installation image from one of the mirrors listed on the OPNSense [website](https://opnsense.org/).

The easiest way to install is from a USB stick using the provided images.

Burn your image to a USB stick, either from dd under FreeBSD, GNU/Linux or Mac OSX or using a physdiskwrite type utility on Windows. Before burning the image, you need to decompress it using bunzip2 or 7-Zip.
### For FreeBSD

`dd if=OPNsense-##.#.##-[Type]-[Architecture].[img|iso] of=/dev/daX bs=16k`

X is the device number of your USB stick (check in dmesg)
 ##### For GNU/Linux

`dd if=OPNsense-##.#.##-[Type]-[Architecture].[img|iso] of=/dev/sdX bs=16k`

X corresponds to the IDE number of your USB key (check with hdparm -i /dev/sdX) (ignore the warning due to the digital signature)
 ##### For Mac OSX

`sudo dd if=OPNsense-##.#.##-[Type]-[Architecture].[img|iso] of=/dev/rdiskX bs=64k`

r = raw device, and X = the device number of your CF card (check with the "diskutil list" command)
 ##### For Microsoft Windows

`physdiskwrite -u OPNsense-##.#.##-[Type]-[Architecture].[img|iso].img`

(use version v0.3 or later!)

## Installation
---

Once you created a bootable USB stick with the downloaded image. Configure your target system BIOS to boot from the USB stick (search online because it depends on the manufacturer).

{{< figure src="/images/tutorials/opnsense/diy-opnsense2.png" alt="opnsense/diy-opnsense" >}} 

According to the official documentation, it has been designed to always boot into a live environment in order to be able to access the GUI or even SSH directly. If a timeout was missed, simply restart the boot procedure.

To trigger the installation type as, login "root" and "installer".

The installation process consists of several steps:

*Note: You will lose all files on the installation disk. If another disk is to be used, you must choose the Custom installation method and not the Quick/Easy Install method.*

- Select the task - the Quick/Easy Install option is ok in most cases. For installation on embedded or custom systems, select Custom Installation- and do not create a swap partition. Continue with the default options.
- Are you sure? - OPNSense installs itself to the first disk in the system.
- Reboot - The system is now ready to be installed and requires a reboot to complete the configuration.

## Initial configuration
---

After being installed, the Opnsense will ask you to choose the interface assignment. If you ignore this step, the default configuration will be applied. The configuration ends with a login prompt.

By default, you must log in to access the console.

Welcome message

```
* * * Welcome to OPNsense [OPNsense 15.7.25 (amd64/OpenSSL) on OPNsense * * *

WAN (em1)     ->
LAN (em0)     -> v4: 192.168.1.1/24

FreeBSD/10.1 (OPNsense.localdomain) (ttyv0)

login:
```
*Note: The default login information is "root" for the user and "OPNsense" for the password.*
### VLANs and interface assignment

If you choose to do a manual installation or when no configuration can be found, you will be asked to assign interfaces and VLANs. If you need a VLAN, then select the "no" option. You can always configure VLANs later.
 ##### LAN, WAN and optional interfaces

The first interface is the LAN interface. Enter a suitable name for this interface (e.g. "usb0", the USB-to-Ethernet adaptater)

The second is the WAN interface. Enter a suitable name for this interface (e.g. "re0"). 

Optional interfaces can be assigned as optional interfaces (OPT). 

If you assign all your interfaces, you can press the "ENTER" button and confirm your settings. OPNSense will configure your system and present you with the login prompt when it is finished.
### Console:
The console has 13 options :

```
0) Logout                      7)  Ping host
1) Assign interfaces           8)  Shell
2) Set interface(s) IP address 9)  pfTop
3) Reset the root password     10) Filter logs
4) Reset to factory defaults   11) Restart web interface
5) Reboot system               12) Upgrade from console
6) Halt system                 13) Restore a config
```
##### OPNsense-update

OPNsense provides a command line interface (CLI) "OPNsense-update". Via menu option 8) Shell, the user can obtain a shell and execute the OPNsense-update command.

For help, type OPNsense-update -help and [Enter].
 ##### Updating from the console

The other way to update your system from the console is via the GUI with the 12) Upgrade from console

An update can also be triggered via the System -> Firmware menu on the web interface.

## Web interface
---
Once eveything is installed, you only have to type in the IP address of your old laptop and voilà :

{{< figure src="/images/tutorials/opnsense/diy-opnsense1.png" alt="opnsense/diy-opnsense" >}} 

## What's next
---
Of course, not everything is working out of the box, for example you will have to :
- Configure WiFi to make an access point
- Setup the powerful Suricata as an *Intrusion Dectection System* (IDS)
- Setup your own network adress plans (for example WiFi network can be on another adress plan than LAN)
- Setup your firewall rules properly
- Don't forget your Unbound DNS server wich is running by default (here's some [ideas](/tutorials/pi-hole-or-how-to-adblock-your-whole-network/))!

The OPNsense [documentation](https://docs.OPNsense.org/) is complete enough to permit myself to leave you right there. If I had to deal with each point one by one I would just copy and paste all the way to the end but it's not the purpose of this article !

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️


 

