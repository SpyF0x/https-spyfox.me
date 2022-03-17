# DMZ : One click to get hacked


By design, your gateway has a set of firewall rules to prevent anyone from reaching directly your devices from the Internet

But this same gateway, has also an option called DMZ to cancel this protection. 

## First things first 
---

Originally, it is a set of rules to ensure that a server will be connected to the Internet, but will not be able to access the local network : 

{{< figure src="/images/security/dmz/DMZ-selfhost-good.png" alt="denial of service attack" >}} 

Your server becomes the front door of your network. 

If the server is vulnerable or if the firewall rules are insufficient to properly isolate the server(s) from the rest of the LAN (which is the case of 99.9% of home routers), any hacker could get into your home network simply by creating a bridge between your server and your home network !

{{< figure src="https://media.giphy.com/media/25KEhzwCBBFPb79puo/giphy.gif" alt="denial of service attack" >}}

##  Story time
---

One day, a friend of mine which was setting up a Minecraft game server, had just discovered the "DMZ" option on his router. He didn't even bother to forward the right port (25565/TCP) and he configured his gateway to set his laptop into DMZ. 

He was pretty proud of it, because it was really convenient : just set up an app on a computer, et voilà ! Don't even bother with "complicated" port forwarding ! 

Even if I strongly recommended him to stop using DMZ to forward only 1 port, he wouldn't listen.

So we played him a little trick with my best friend :

> 1. He was running GNU/Kali Linux on his laptop. We knew his username (it was his IG nickname) as well as his password, as he was reusing the same damn password everywhere.

> 2. A quick port scan with Nmap showed a whole bunch of open ports, including SSH.

> 3. We tried to connect to his laptop through SSH...and it successfully worked ! 

The biggest question was, "What do to now ?"

Tbh, I can't remember exactly what we did next. I'm pretty sure that we had overloaded his desktop folder with blank files and changed his wallpaper to a picture of his desktop actually being full of blank files so that he can't remove those files because it's a part of his wallpaper. 

Trust me, his live reaction when this happened was priceless !

{{< figure src="https://media.giphy.com/media/8Iv5lqKwKsZ2g/giphy.gif" alt="denial of service attack" >}}

## Conclusion of the story 
---

Keep in mind that all this was just a friendly joke, and our friend took it very well. A real cyberattack would have had catastrophic impacts. 

We can imagine that instead of scaring our friend, we could have set up a second backdoor, even more discreet to be more persistent and from there, reroute all our traffic to our friend's LAN. After all, all his router is doing is simply exposing all his server's ports, there is no firewall rule that would prevent us from doing so.

At the time when we made the joke, Eternalblue / DoublePulsar had just been released, it was a critical flaw impacting Windows boxes that allows an attacker to execute arbitrary code remotely. We would have used this flaw to have a firm foothold on all the Windows boxes of his network and so on until all his entire network would be compromised ! 

Even for a simple individual, the impact can be huge : imagine one day turning on your computer and see a cryptolocker. 

Turning on your laptop to look for the solution online and see that your laptop as well as all the other computers in your house are cryptolocked ! 

{{< figure src="https://media.giphy.com/media/hyyV7pnbE0FqLNBAzs/giphy.gif" alt="denial of service attack" >}}


What's even scarier is that thousands of companies around the world have woken up in this state. The root cause of the initial intrusion is always varied, but in the end, the result is always the same. 

__This is why in my opinion, IT security of companies AND individuals must NOT be traded for convenience !__

## Solution in a nutshell
---

To my eyes, there are two cases : 

- The first, you are at home : 

> Just port forward the needed port(s) with the right protocol (TCP/UDP) and clear that rule when you're done.

> Consider putting your server in a subnetwork or an isolated network from your home network to prevent a bridge in case of a successful attack.

> ⚠️ Don't EVER dare enable DMZ ⚠️ 

> Unless, of course, if you want to get hacked after approximately 38 seconds by an automated bot attack !

> Also, keep your server up to date.

- The second you are an IT admin : 

> Properly get your firewall rules checked by different persons. There's no shame to verify something by someone else !

> Run some pentests in black and gray boxes.

> Dedicate some resources to maintain the servers up to date. 

> ✅ It is critical to eliminate vulnerabilities as soon as a patch is available !

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
