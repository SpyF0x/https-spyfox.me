# Setup your own DNS server with ease


You may not be aware of it, but every time you type a URL in the address bar of your browser, it is transmitted in clear text to the DNS server you are using. 


## A bit of background
---
The latter then takes care of resolving the domain name, and tells your machine how to connect to the right server that will distribute the expected data.

But here's the problem : DNS is a 40 years old protocol, and at that time engineers were not too sensitive to privacy issues. So the overall confidentiality of DNS is nearly inexistant. 

In most cases, DNS requests are made in clear text. Thus, an entity located between you and the DNS server has the complete ability to know which websites you are visiting.

However, solutions do exist such as DNS-over-TLS and DNS-over-HTTPS, which allow DNS requests to be encapsulated in encrypted protocols. Unfortunately, they are still not widely implemented.

## What to do ?
---
Even if you're a complete beginner and don't understand much about it, just follow this dead simple tutorial.

The idea is simple... Since our ISP's DNS servers are lying and the public ones are maintained by fotune 500 companies interested into our juicy browsing data, the most logical solution would be to run our own DNS server inside (or outside) our home network. 

I first came across SimpleDNS Plus, a Windows software that allows you to do this easily, except that it's only for Windows and that you have to pay for it, so shame on it.

I obviously thought about the powerful Bind 9, but the configuration is quite heavy, so it's not ideal to make a tutorial accessible to anyone. 

While browsing the internet, I saw that there was a tool called Unbound that allows you to do this on Linux as well as Windows and OSX.

The main benefit is that the basic configuration is more than enough to get a properly configured DNS server. 

*Note: As Unbound is a server, I would recommend you to install it on the machine wich is turned on the most of the time. In this tutorial, I will cover the installation for Linux, BSD, OSX and Windows OS*

## Windows OS (Desktop & Server)
---
{{< figure src="/images/tutorials/dns/windows.jpg" alt="a wonderful Windows 10 meme image" >}} 

First, go download Unbound DNS [here](https://unbound.net/download.html). 

That's quite easy for Windows, because it's a very traditional installer. No need to think about it, just go to the end of the installation and leave all the default settings.

When finished, you will see in the list of Windows services (Start -> Run -> Services.msc), an Unbound service that is supposed to be started and running automatically. If not, configure it to start automatically and run it.

You'll have to configure your Windows to use your local DNS server, whose address will be 127.0.0.1. Go to the network settings and put 127.0.0.1 in the DNS field (and nothing in the second DNS field). And that's it for Windows !

To check if it's working, launch a terminal and execute "nslookup". If you use it to resolve some domain names, you should see something like this:

```
C:\Windows\system32> nslookup spyfox.me
Server: localhost
Address: 127.0.0.1 

Name: spyfox.me
Address: 104.21.17.108
```

We can see here that spyfox.me is successfully resolved through our local DNS 127.0.0.1.

Finally, launch your browser and enter the URL of your choice. The URL may not be resolved right away. This is quite normal as it is the first time it is loaded this way. 

For the next times, as the request will already be cached the resolution will be lightning fast.

## UNIX-like (GNU/Linux / BSD / OSX)
---
{{< figure src="/images/tutorials/dns/unix.png" alt="a funny jurassic park UNIX meme" >}} 

For Linux, OSX and some BSDs, you can of course use your favorite package manager (apt, yay, xbps-install, pkg, macport...) to install Unbound directly on your system.

However, I advise you to run it in a docker container instead. This way it will be a bit more secure and convenient : 

First pull the image : 

```
$ docker pull mvance/unbound
```

Then run the container with the following command :
```
$ docker run --name my-unbound -d -p 53:53/udp -p 53:53/tcp \
--restart=always mvance/unbound:latest
```

*Note: Don't forget to change your DNS servers settings on your computer ! Think about how your system is handling that (e.g. `/etc/resolv.conf`)*

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

