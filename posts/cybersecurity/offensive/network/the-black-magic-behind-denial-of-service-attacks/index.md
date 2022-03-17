# The black magic behind denial of service attacks


A denial of service attack is when an application (or anything else) is prevented from doing what it was designed to do.



## A little bit of context
---
Denial of service (or DoS) attacks are back in the news every year. Whether it's with the breakdown of Xbox live, Playstation Network or Steam servers, the goal is always the same: 

to prevent an application or a server from doing its job.

There are basically two types of denial of service attacks :

- Low-traffic attacks
- High-traffic attacks

But before detailing the techniques that can be used by hackers to achieve their needs. One may wonder why they do these attacks and annoy so many people.

## Why conduct a denial of service attack
### To protest

Anonymous and other hacktivist groups use denial of service attacks to paralyze a website, just as one would protest in the street.

{{< figure src="/images/security/denial-of-service/anonymous_ddos.jpg" alt="denial of service attack" >}} 

A website can be inaccessible for a few minutes / hours / days. It all depends on the duration of the attack. 

It has been the case with Lulzsec taking down MasterCard and VISA websites to protest to the increasing pressure of the governments to control the Internet.
### Diverting attention

Hackers can also use this type of attack, which is very visible, to divert the attention of the security experts of the company they are attacking. 

In parallel, they will execute one or more, other quieter attacks to break into the company's network.

{{< figure src="/images/security/denial-of-service/denial-of-service1.webp" alt="denial of service attack" >}} 
### Social engineering

A hacker may also want to stop an application after introducing himself to the employees as the person to contact in case of malfunction. 

The employees will then trust the hacker and be inclined to give him confidential information.
### Information leakage

Generally, when an application stops working, the persons in charge will try to restart it. 

During this restart stage, information that would normally be inaccessible could be visible :

{{< figure src="/images/security/denial-of-service/denial-of-service2.webp" alt="denial of service attack" >}} 

- File list
- Identifiers
- Computer names
- etc.

This information leakage can also occur if a hacker manages to stop a part of a server without altering the whole. 

For example, TLS encryption can be stopped to intercept information in plain text, or an SSH server can be stopped to force administrators to connect using an unencrypted telnet connection.

## Attacks methodologies 
### ICMP Flood

It's one of the oldest attacks. The attacker saturates the target with ICMP echo requests (a kind of ping) or other types, which allows to bring down the victim's network infrastructure.

{{< figure src="/images/security/denial-of-service/ICMP-flood-attack.png" alt="denial of service attack" >}} 
### SYN Flood

Is another classic. It consists in initiating TCP connections to the server with a SYN message. The server takes this message into account and replies to the client with the SYN-ACK message. 

The client must then respond with a new ACK message to tell the server that the connection is successfully established. 

{{< figure src="/images/security/denial-of-service/SYN-flood-attack.png" alt="denial of service attack" >}} 

However, in the case of a SYN attack, the client does not respond and the server keeps the connection open. 

By multiplying this kind of SYN messages it is possible to paralyze the server.
### UDP Flood

It consists in sending a large number of UDP packets to random ports of the target machine. 

{{< figure src="/images/security/denial-of-service/UDP-flood-attack.png" alt="denial of service attack" >}} 

The latter then indicates with an ICMP message that no application is responding on this port. 

If the quantity of UDP packets sent by the attacker is significant, the number of ICMP messages sent back by the victim will also be significant, saturating the resources of the machine.
### Amplification attacks

Amplification attacks involve spoofing the victim's IP address to send small queries to a server but receiving a much larger response from it. 

{{< figure src="/images/security/denial-of-service/amplification-ddos-attacks.jpg" alt="denial of service attack" >}} 

A botnet can then greatly amplify the size of an attack by relying on existing legitimate infrastructure such as DNS or NTP servers. 

Here is a small summary table showing the protocols offering the most amplification : 

{{< figure src="/images/security/denial-of-service/amplification-ddos-attack.webp" alt="denial of service attack" >}} 
### Exploiting the limits of the machines

There are a plethora of possibilities to make a denial of service attack. Any loophole can be used. 

You have certainly already seen this type of message :

{{< figure src="/images/security/denial-of-service/win_ddos.png" alt="denial of service attack" >}} 

A DoS attack consists in obtaining the same result remotely. All means are good:

- Buffer overflow
- Fills the disk space
- Consume all available memory or CPU time
- etc.

One of the best known examples of application layer attack is Slowloris, a tool developed by RSnake (Robert Hansen) that keeps connections open on the target server by sending a partial request. These accumulated unclosed connections completely saturate the server in several minutes.

Apart from Slowloris, tools such as Low Orbit Ion Cannon (LOIC), High Orbit Ion Canon (HOIC), R.U. Dead Yet? (RUDY), #RefRef or hping also allow, without much knowledge, to perform DoS attacks without having to use huge botnets. 

For example, #RefRef exploits a vulnerability present in SQL databases, and thanks to an SQL injection of a few lines, is capable of triggering an effective denial of service.

{{< figure src="/images/security/denial-of-service/refref-dos.jpg" alt="denial of service attack" >}} 

## Distributed attacks

DDoS (Distributed Denial of Service) is a denial of service attack which, as its name indicates, is distributed. 

This means that several tens / hundreds / thousands of computers, grouped in what is called a "botnet", will simultaneously attack a website, relying on the number of connections to make the application inaccessible. 

Here is a real life example :

{{< figure src="/images/security/denial-of-service/china_traffic_jam_ddos.jpg" alt="denial of service attack" >}} 
*China National Highway 110 traffic jam*

To properly operate the botnet, a hacker would need a command and control server (C&C or C2). 

It is basically the keystone of the whole infrastructure. It is to this central server that the botnet obeys. If this server disappears, it is the entire botnet wich disappear.

{{< figure src="/images/security/denial-of-service/botnet_ddos.png" alt="denial of service attack" >}} 

## Dyn DDoS

On October 2016, Dyn fell victim to a massive DDoS attack of more than a Terabyte per second (the equivalent of 220 bluray per seconds). 

Many websites  using the Dyn Managed DNS service (different from DynDNS), such as Twitter, Spotify, Soundcloud, Reddit, Ebay, Netflix, GitHub, PayPal, are inaccessible for about ten hours. 

It has been mentioned that the attackers used hacked connected objects (such as surveillance cameras) to relay the massive flow.

{{< figure src="/images/security/denial-of-service/dyn_ddos.png" alt="denial of service attack" >}} 

Note that in November 2016, as a direct consequence of the attack, Dyn was acquired by Oracle for an undisclosed amount.

Of course, no one is immune to a DDoS attack. 

Fortunately, there are technical solutions that can detect and reduce the impact of a high-traffic attack but only backups and redundancy would help against low-traffic attacks.

To get a better idea of how common these attacks are, digitalattackmap.com has an animated map. 

You can see the nature, origin and destination of such attacks that are taking place as you read this.

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
