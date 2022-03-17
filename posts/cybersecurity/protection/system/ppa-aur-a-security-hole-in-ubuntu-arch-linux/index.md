# PPA & AUR : A security hole in Ubuntu & Arch Linux ?


Arch Linux and Ubuntu are both excellent distributions whose security is taken very seriously by their people, but these two behemoths both have a BIG Achilles heel.


## First things first
---
Don't get me wrong folks, I really like the philosophy behind those two distributions : 

- Making Linux accessible to anyone for Ubuntu
- Pushing customization to the limit for Arch Linux (as long as we have nothing against systemd)

I also spent a lot of time on these two distributions using them as daily drivers, and they are both great !

*So why this article would you tell me ?*

Well, in fact the idea has been on my mind for some time, but a recent discussion with a friend of mine who uses Arch Linux with over 95% of his everyday software coming from AUR convinced me to warn you about the potential problems with this type of repository.

## Some generalities 

Every GNU/Linux (and some BSDs) has its own philosophy for package management. That is to say : 
>*"How to offer to the user, a simple way to properly install a program (including its dependencies) and remove it without messing up all the operating system."*

While APT, Aptitude and DPKG have been the answer provided by Debian team, only Pacman and ABS has been chosen by the Arch Linux one. 

For your general knowledge, you can check this pretty well done ["List of software package management systems"](https://en.wikipedia.org/wiki/List_of_software_package_management_systems) from Wikipedia.

As each package manager system uses its own set of rules to comply with its distribution philosophy, all of these solutions are fundamentally different when dealing with the updates or the dependances for example. 

## Arch Linux - Arch User Repository (AUR)

{{< figure src="/images/security/security-aur-ppa/security-aur-ppa0.png" alt="a cool image" >}} 

From the official Arch Linux Wiki : 
*[Pacman]"combines a simple binary package format with an easy-to-use build system. The goal of pacman is to make it possible to easily manage packages, whether they are from the official repositories or the user's own builds.*

*Pacman keeps the system up-to-date by synchronizing package lists with the master server. This server/client model also allows the user to download/install packages with a simple command, complete with all required dependencies."*

By default, only official repositories are enabled in Pacman, but the official Arch Linux Wiki shows you how to enable AUR easily.

One thing I really like about the official documentation is that it is pretty well done, it gives you all the explanations about what you are doing and even gives you warnings from the beginning : 

{{< figure src="/images/security/security-aur-ppa/security-aur-ppa2.png" alt="a cool image" >}} 

And continues to warn you throughout the article...

{{< figure src="/images/security/security-aur-ppa/security-aur-ppa3.png" alt="a cool image" >}} 

What I think is a shame however, is that the most tutorials I've found on the internet don't do the same. Of course, there are some good people who warn you, but unfortunately too few do it as well as the documentation.
### What's wrong with AUR ? 

The packages offered on AUR are under the full responsibility of the users. So, if someone wants to share a backdoor with the whole community, guess what ? He absolutely F'ing can ! 

Moreover, the "types" of software offered are more numerous, some packages retrieve the sources of a software to compile them, others are only converters of deb packages into xz packages, some install dependencies from the standard repositories of the distribution, others use other dependencies on AUR, free software, open-source software, proprietary software... 

In short, it's very powerful, but it can quickly become a big mess. Especially when a user drops his package, which can turn out to be broken for different reasons, mainly because Arch Linux is constantly evolving (e.g. package names, versions, etc.).

## Ubuntu - Personal Package Archives (PPA)

{{< figure src="/images/security/security-aur-ppa/security-aur-ppa4.png" alt="a cool image" >}} 

In the case of Ubuntu, PPA's are used to include a specific software to your system which is not included in the official Ubuntu repos for some reasons (e.g. deprecated, licences, etc.). 

You have to see a PPA like a repository where a user can put one or multiple program but absolutely not as a massive community driven repo as AUR. 

For example, Microsoft have their own PPA to distribute Microsoft Edge or Visual Studio code : 

{{< figure src="/images/security/security-aur-ppa/security-aur-ppa5.png" alt="a cool image" >}} 

That's why, to my eyes, the "safeness" of a PPA mostly depends on two basic questions I would wonder :

- Do I trust the owner ? 
- How often the packages are updated ?

If the answers to these two questions satisfy me, then I don't see any problem to use the ppa in question to install my weird software which has no place in the [33,000](https://repology.org/repositories/statistics) packages proposed in the official repositories of Ubuntu !

## Conclusion

If what you want to install is not officially supported by either the Ubuntu or Arch Linux teams, there is some good reasons. 

{{< figure src="https://c.tenor.com/UbJAmff1ddUAAAAd/dwight-rainnwilson.gif" alt="a cool image" >}} 

So, before connecting your favorite system to a repository that could very quickly become your worst nightmare, read up on the software you want to install, find out why it is not available and most importantly, find alternatives ! 

Remember why you are using GNU/Linux today, it's because one day, you found this alternative to Windows or OSX 🤠

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
