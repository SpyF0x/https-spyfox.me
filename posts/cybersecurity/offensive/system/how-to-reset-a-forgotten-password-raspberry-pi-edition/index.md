# How to reset a forgotten password (Raspberry Pi edition)


So today, we're going to look at how to hack your own Raspberry Pi in order to break any system authentication.


## A bit of background
---
Last week, I wanted to make sure that my favorite device, a proud Raspberry Pi, was secure because there was an important vulnerability on a software I knew was installed on it. 

Happy to open a terminal in order to connect via SSH to this little wonder, my pleasure was interrupted when I saw this : 

{{< figure src="/images/security/raspberry-passw/raspberry-passw1.png" alt="denial of service attack" >}} 

At the time, I thought I must have missed a character, a capital letter, or even a numbers. So I rewrote my password, being sure of what I was writing, and my favorite mini-computer answered me this : 

{{< figure src="/images/security/raspberry-passw/raspberry-passw1.png" alt="denial of service attack" >}} 

Until the famous final third attempt where the server, doing its honest job, happily closes the connection surely thinking that I must be one of those countless brute force bot that roam the internet trying to find some poorly protected computers. 

I have to tell you that I usually just do a global unplugging where I reformat the whole sd card to have a clean image on it again. 

But as this raspberry was, let's say, a rather important part of my home network, reformatting it was simply out of the question. 

So, I had to find an alternative to my usual brutality. Today, I'll show you this miracle solution that saved my weekend.
## The Idea

There are several authentication schemes that can be used on GNU/Linux systems but the most commonly used and standard scheme is to perform authentication against the `/etc/passwd` and `/etc/shadow` files.

The first one is a plain text-based database that contains information for all user accounts on the system. It is owned by root and has 644 permissions, meaning that the file can only be modified by root or sudo users, but it is readable by all system users.

The second one, on the other hand, is only accessible by root or sudo users because it contains all the system’s users passwords.

Don't get too excited, those passwords are heavily encrypted, so don't expect to get your password and guess in the process your whole user's just by looking inside this file !

Nope, to recover our password, we will simply tamper this file to replace our encrypted password with a temporary known one, that we know the clear version. 

## Tampering etc/shadow

To do that, simply take the sd card out of your Raspberry Pi and put it in your SD-Card reader (or SD-USB adapter if you don't have one).

Once everything is mounted, you should get a similar output to the lsblk command : 

```
$ lsblk 
NAME    MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sdb       8:16   1   7,4G  0 disk  
├─sdb1    8:17   1   256M  0 part  /media/spyfox/boot
└─sdb2    8:18   1   7,1G  0 part  /media/spyfox/rootfs
```

As you can see, we have two partitions on our SD-Card. Head to the Rootfs, you shoud see something like this : 

```
$ cd /media/spyfox/rootfs
bin   dev  home  lost+found  mnt  proc  run   srv  tmp  var
boot  etc  lib   media       opt  root  sbin  sys  usr
```

I think we've unlocked the "Linuxception" success 🤠

{{< figure src="/images/security/raspberry-passw/raspberry-passw3.png" alt="denial of service attack" >}} 

Before tampering the `shadow` file, we will generate a known temporary password only to connect via SSH. Once connected we will reset our password following the standard procedure because by principle I don't trust my computer.

To do so, I've searched a lot and I found this solution using Python3 : 

```
python3 -c "from getpass import getpass; from crypt import *; \
    p=getpass(); print('\n'+crypt(p, METHOD_SHA512)) \
    if p==getpass('Please repeat: ') else print('\nFailed repeating.')"
```

This solution has the following benefits:

- Nothing additional to install (Python 3 is widely supported)
- Does not store the password in your shell history.
- Generates a random salt for you.
- Uses a modern, strong hashing algorithm (SHA-512)
- Re-prompts for the password to avoid mistakes.

The next step is quite simple, because you just need to copy / paste the new hash to the `etc/shadow` file to override the previous one.

_/!\ CHECKPOINT : MAKE SURE YOU ARE EDITING `etc/shadow` AND NOT `/etc/shadow` !!!_

Once `etc/shadow` has been properly tampered, just put back the SD-Card in the raspberry, and we are good to go !

## Just to be sure

As soon as you are logged back in with your previously blocked user, and you are under total admiration of your SSH banner :

{{< figure src="/images/security/raspberry-passw/raspberry-passw2.png" alt="denial of service attack" >}} 

You should properly reset your user password with a new one with this simple command : 

```
$ passwd 
```

And you should be good ! 

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
