# Secure your GNU/Linux server with these few iptables rules


The simple fact that your server is accessible on the Internet means that it is being attacked every 39 seconds on average. 

That is more than 2,215 times in a day !

Should we simply stop having servers ? Of course not !  



## Why such attacks ?
---
When I say this kind of things (which are actually [true](https://enme.umd.edu/news/story/study-hackers-attack-every-39-seconds)) many people take it as a joke or worse, are skeptical :

> "Why would my Minecraft server, where only my friends and I got access, get attacked every 39 seconds ?" 

First, this type of server, and even more when it is self-hosted, is very valuable because of its hardware. A game server provides a lot of computing power that any hacker can turn into significant crypto mining capabilities.

Moreover, the fact that only you and your friends know the IP address, doesn't change the problem at all, since it is on the internet. So all the goddamn planet have access to it !

The same applies for a web server or a file server.

Regarding IOT devices (weather station, IP camera, etc.), does the word "[Mirai Botnet](https://en.wikipedia.org/wiki/Mirai_(malware))" sounds familiar to you ?

TL;DR : For money. 

## What solution ?
---

A [firewall](https://en.wikipedia.org/wiki/Firewall_(computing)), which will act as a network filter. 

More precisely, a firewall rule (e.g. "_deny the access of port 22 from Internet_"). 

Once configured, the firewall will be applying, for example, this rule and your SSH port (22) will be closed from the Internet point of view :  

```
$ nmap 82.70.85.2 -p 22

Nmap scan report for random.internet (82.70.85.2)
Host is up (0.00052s latency).
Not shown: 65534 closed ports
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
```

But not from your (home) point of view : 

```
$ nmap 192.168.1.58 -p 22

Nmap scan report for server.home (192.168.1.58)
Host is up (0.00045s latency).
Not shown: 65534 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
```

As you can see, this "network filter" bears its name well. 

I actually used "iptables" which is an interface for "[Netfilter](https://en.wikipedia.org/wiki/Netfilter)", a GNU/Linux kernel module to handle network filtering. The goal of this article will be to show you how you can do so.

## How ?
---

When I have to create firewall rules, I need to properly formulate my needs. 

That's why in the first place, I write in human language what I want :

1. Deny all connections by default (the least privilege principle).
2. Accept only communications to the chosen ports (80,443 for web server).
3. Block SSH to everyone but us.
4. Allow the server to communicate with itself (important for some programs).
5. Allow the server to respond back to ESTABLISHED connections.

And now create a file :

```python
*filter

# Allow traffic on loopback
-A INPUT -i lo -j ACCEPT

# Allow ESTABLISHED and RELATED packets
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

# Allow incoming TCP traffic on specified ports (ex: web server)
-A INPUT -p tcp --dport 80 -j ACCEPT
-A INPUT -p tcp --dport 443 -j ACCEPT
# ---------- INSERT SSH RULE HERE ---------- #

# Reject traffic other than for the rules mentioned above
-A INPUT -j DROP

COMMIT
```

## The SSH problem
---

SSH is a big problem because it is essentially a paradox in our firewall rules. 
Indeed, we want nobody except us to administrate our server BUT we want to do it whenever we want.


In my opinion, there's two situations : 

- A . You can afford a static IP address. Add this rule :

```python
# Allow SSH for specified IP (whitelist)
-A INPUT -p tcp --dport 22 ! -s 1.2.3.4 -j DROP
```

- B . You have a dynamic IP address as the rest of the world. 

The solution is to move SSH to a non-standard port, you will most likely the only one to know :

Add "Port 45632" for example to SSH config `/etc/ssh/sshd_config` and reload your SSH daemon. 

Then add this rule : 

```python
# Allow incoming TCP traffic on port 45632  
--A INPUT -p tcp --dport 45632 -j ACCEPT
```

## Apply iptables rules
---

Once you're good, do this command : 

`# iptables-restore > path-to-your-file`

Then verify that everything has been properly applied : 

`# iptables -L`

## CTRL + Z
---

If you want to undo everything : 

`# iptables -F`

/!\ Don't hesitate to bookmark my post, so you have everything in one place !

## Make a good action ! 
---

Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
