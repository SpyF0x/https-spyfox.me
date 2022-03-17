# Pi-hole or how to adblock your whole network


Pi-hole is a powerful network-level ad blocker that acts as a local DNS and possibly as a DHCP server. You can easily block ads on all your devices (TV, computers, phones, etc.)


## A bit of background
---
The Pi-hole project was created by Jacob Salmela as an open source alternative to AdTrap in 2014 and was hosted on GitHub. Since then, several contributors have joined the project, including dschaper, PromoFaux and DL6ER. 

It is designed to be installed on embedded devices with networking capabilities, such as the Raspberry Pi, but it can be used on other machines running Linux or in virtualized environments. 

## Facts about Pi-hole : 
---
- Open source software (under European Union public license)
- Selfhosted (you keep your data)
- Intuitive web interface (you can disable it)
- Easy to install
- Support of Raspberry PI and docker
- Easy on CPU / Memory

## One-Step Automated install: 
---
Those who want to get started quickly and conveniently may install Pi-hole using the following command:

```shh
# curl -sSL https://install.pi-hole.net | bash
```

_[Note]()  :_
_Piping to bash is a controversial topic, as it prevents you from reading code that is about to run on your system._
_If you would prefer to review the code before installation, we provide these alternative installation methods._

## Alternative 1: Clone the repository and run

```sh 
$ git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
$ cd "Pi-hole/automated install/"
$ sudo bash basic-install.sh
```

## Alternative 2: Manually download the installer and run
---
```sh
$ wget -O basic-install.sh https://install.pi-hole.net
$ sudo bash basic-install.sh
```

## Alternative 3: Use Docker to deploy Pi-hole
---
See the Pi-hole docker [repo](https://github.com/pi-hole/docker-pi-hole) to use the Official Docker Images.

After any option, all you have to do is let yourself be guided by the automated installer. Just note the password at the end of the process, so you can log into the web interface. 

But don't worry, if you forget it you can simply reset it by typing : 

```sh 
$ pihole -a -p
```
## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

