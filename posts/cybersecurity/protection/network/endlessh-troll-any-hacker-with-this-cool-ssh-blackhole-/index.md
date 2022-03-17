# Endlessh : Troll any hacker with this cool SSH blackhole !


Firewalls, brute force detection, SSH on random port, geoip filtering, honeypots... 


There are LOTS of ideas to make life hard for the bot army that constantly probes the internet. 

Well, thanks to "skeeto" you can add "Endlessh" to this list !

## How does it work ?
---


According to the official Github [repository](https://github.com/skeeto/endlessh), Endlessh is an SSH tarpit that very slowly sends an endless, random SSH banner. 

It keeps SSH clients locked up for hours or even days at a time. The purpose is to put your real SSH server on another port and then let the script kiddies get stuck in this tarpit instead of bothering a real server.

Since the tarpit is in the banner before any cryptographic exchange occurs, this program doesn't depend on any cryptographic libraries. It's a simple, single-threaded, standalone C program. It uses poll() to trap multiple clients at a time.

## Installation
---

Endlessh is a simple C program of ~800 LoC (Lines of Code). A simple "make" after a git clone and you're good to go !

In case you're wondering, a template unit file can be found in [util/](https://github.com/skeeto/endlessh/blob/master/util/endlessh.service) folder to allow you to start it as a Systemd daemon.

## Usage
---

Once compilation is complete, you will most likely want to create a config.txt file like this one : 

```python
# The port on which to listen for new SSH connections.
Port 22

# The endless banner is sent one line at a time. This is the delay
# in milliseconds between individual lines.
Delay 10000

# The length of each line is randomized. This controls the maximum
# length of each line. Shorter lines may keep clients on for longer if
# they give up after a certain number of bytes.
MaxLineLength 32

# Maximum number of connections to accept at a time. Connections beyond
# this are not immediately rejected, but will wait in the queue.
MaxClients 4096

# Set the detail level for the log.
#   0 = Quiet
#   1 = Standard, useful log messages
#   2 = Very noisy debugging information
LogLevel 0

# Set the family of the listening socket
#   0 = Use IPv4 Mapped IPv6 (Both v4 and v6, default)
#   4 = Use IPv4 only
#   6 = Use IPv6 only
BindFamily 0
```

And run Endlessh : 

```
./endlessh  >endlessh.log 2>endlessh.err -f config.txt
```

## Endlessh in action !
---

I've ran it over the weekend on one of my server and I've managed to trap some bots so far ! 

See my Endlessh.log file content : 

```python
...
23:05:28 Port 22
23:05:28 Delay 10000
23:05:28 MaxLineLength 32
23:05:28 MaxClients 4096
23:05:28 BindFamily IPv4 Only
23:05:28 socket() = 3
23:05:28 setsockopt(3, SO_REUSEADDR, true) = 0
23:05:28 bind(3, port=22) = 0
23:05:28 listen(3) = 0
23:05:28 poll(1, -1)
23:05:44 = 1
23:05:44 accept() = 4
23:05:44 setsockopt(4, SO_RCVBUF, 1) = 0

# -------- Chinese hacker bot trapped ! --------#

23:05:44 ACCEPT host=183.149.238.211 port=58884 fd=4 n=1/4096 

# Begining sending empty lines forever with a 10 seconds interval...

23:05:44 poll(1, 10000)
23:05:54 = 0
23:05:54 write(4) = 32
23:05:54 poll(1, 10000)
23:06:04 = 0
23:06:04 write(4) = 16
23:06:04 poll(1, 10000)
23:06:14 = 0
23:06:14 write(4) = 32
23:06:14 poll(1, 10000)
23:06:24 = 0
23:06:24 write(4) = 13
23:06:24 poll(1, 10000)
23:06:34 = 0
23:06:34 write(4) = 26
23:06:34 poll(1, 10000)
23:06:44 = 0
23:06:44 write(4) = 21
23:06:44 poll(1, 10000)
23:06:54 = 0
23:06:54 write(4) = 6
23:06:54 poll(1, 10000)
...
```
I really think Endlessh could be a more friendly and a more legal way to annoy these hackers than directly hack them back as I've already done in this [article](https://spyfox.me/stories/an-iso-file-a-botnet-a-world-of-warcaft-server-and-me/).

That's it ! Don't hesitate to bookmark my post, so you have everything in one place !

## Make a good action ! 
---

Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

