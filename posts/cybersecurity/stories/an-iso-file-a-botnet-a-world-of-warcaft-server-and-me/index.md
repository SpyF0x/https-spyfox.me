# An ISO file, a botnet, a World of Warcraft server and me


Here is one of my many hacking stories originally posted on my previous website. Please note that all the images in this post are coming from the internet and are here for illustration.



## A bit of background...

A couple of years ago, one of my friend showed me a suspicious email he received at work. As you know me, it was only a matter of seconds before I got the perfect kit of the FBI computer forensics guy.

## The email

It was a classic shaped email sent around 10 am to a medium sized company. 

The customers were used to address their requests through emails to the company so it was perfectly normal for an employee to receive an email with a file attached to it. 

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me.png" alt="a cool picture" >}} 

In addition to that, the sender was already known as a good customer to the company. 

So, *what did stop my friend from opening the attachment ?* 

You may have guessed it, ISO files are generally enormous compared to small traditional PDF attachements as they contains a lot of files and folders. 

For instance, if you install an operating system there are chances that you have download an ISO image of several Gigabytes (or *only* some hundreds of Megabytes if you install a lightweight Linux distribution). 

But here, the ISO file would fit in less than a 1Mb disk space. 

A little bit suspicious isn't it ? 

Futhermore, as I previously mentionned, ISO images are generally used to install operating systems but here, the attachment seems to be a notice or some sort of document.

## The analysis

Being for the first time confronted to this kind of attachment I've decided to inaugurate my brand new lab infrastructure by firing up all my equipments. Quickly the packet sniffer, the fake DNS and the Windoze victim system was up and running.

After a simple double click, the ISO image was mounted onto my system. After some clicks into the newly created folder nothing really happened. 

Finally, there was no virus at all.

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me6.jpg" alt="a cool picture" >}} 

---

Oh wait, what's that ? Look at Wireshark :

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me1.jpg" alt="a cool picture" >}} 

*What the heck is that ?*

I quickly understand that the ISO file was used as an archive for the embeded backdoor. Internet Relay Chat (IRC) was then used to communicate with the Control and Command server (C2C). As IRC is not an encrypted protocol, the login used to log into the botnet was clearly visible on my network capture. I had all the informations I would need to infiltrate the botnet. 

Clearly determined to put an end to the shady business of this hacker (or group of hackers), I decided to take a quick look just to satisfy my curiosity and know how does it feel to infiltrate a botnet.

This is close of what I saw this day: 

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me2.jpg" alt="a cool picture" >}} 

There was a whole bunch of computers. I was surprised to see how many servers this shady guy got under his control because there didn't seem to be any automatic  module inside the backdoor. Only a reverse shell. The hacker might have dumped some files from his victims and access to the servers later. 

Among the zombie servers there was a lot of game servers including a popular World of Warcraft (WoW) server.

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me3.jpg" alt="a cool picture" >}} 

*Actual footage of a hacker turning a public WoW server into a nightmare for players (and admins)*

## The botnet's demise or how to make a good action

After mesuring the extent of the botnet I decided with my friend to report it to the police. 

Indeed, even if I successfully infiltrated the botnet, I was unable to run administrative commands because I had the zombie permissions only. And even if I had, releasing all the zombies would have attracted the attention of the hacker without stopping him from infecting other computers. In fact, reporting to the police was the only way to stop this scam campaign and free once for all the entire botnet.

That's how days after days, after coming back from work I was checking if the botnet was still up. But days after days there was no sign of any takedown. I was a little bit disappointed but after a month or so my console showed me this result: 

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me4.png" alt="a cool picture" >}} 


Generally, in IT, timeouts are very badly seen. But here, it was the very cristalisation of hope. I remembered a blank Apache web server was running on port 80 (probably to serve as a web repository). All excited, I quickly visited the web server.

I was facing a timeout error. In fact it was the entire server wich went out of time.

My friend, being a passionate WoW player, got the (*good ?*) idea to play on the previously zombified server. He managed to have a talk with a member of the staff. 

Apparently they were targeted with a classic DDOS blackmail: if they wouldn't pay, the shady hacker would overload the server with tons of requests, making the game totally unplayable. 

They were sick of those attacks because it was really annoying for anyone (except the hacker) and they were  really struggling at how to mitigate those DDOS attacks without spending too much money.

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me5.jpg" alt="a cool picture" >}} 


In my opinion, paying him wouldn't have changed anything. Here's why: 

If the hacker targeted other people, the Wow server bandwidth would have been saturated anyway because it would have been shared between the legitimate traffic from the players and the traffic to serve the attacks. 

The only way for the hacker would have been to completely uninstall his backdoor but hey, he wouldn't be so fool. 


## Conclusion 

*Communicate !!*

Do you remember the email received by my friend ? It was originated from a trusted client... which has been aware of an intrusion about a week before the phishing email. 

In fact, for image concerns this client had decided not to say anything but when my friend has issued him a warning about the mail he admited the intrusion... cool, but it was already too late !

{{< figure src="/images/stories/wow/An-ISO-file-a-botnet-a-World-of-Warcraft-server-and-me6.gif" alt="a cool picture" >}} 


In my opinion, the next important thing had to report the botnet for takedown. Don't act on your own as it could have dramatic consequences. 

Imagine an increased security of the botnet and thus a more difficult takedown or worse : if you take the leads on the botnet and you get caught.

In the eyes of the law you would be the puppet master and the real bad guy would still be in the wild.

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️


