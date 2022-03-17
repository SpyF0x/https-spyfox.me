# How to know if you are being spied on by your FBI agent

Is someone reading your messages ? How can you be sure that no one is viewing the documents you store on your computer ? 


These are some questions that may be lingering in your head and may lead you to think that someone is spying on you, lurking in the shadows, waiting like a lioness to pounce on her prey

But how to be sure ? Here come [Canary Tokens](https://www.canarytokens.org)

## First things first
---
For those who don't know the concept of the canary yet, here's a quick explanation: a canary is basically a marker, it could be a link, a word, a token, a sentence, an image, even a PDF file, that you put somewhere on the internet (e.g. chat message, email, etc.). 

If someone else other than you triggers the marker, you know for sure that someone is eavesdropping.

Let's say, for instance, Bob and Alice are very close friends, and they want to make sure that no one else is watching their online conversation. They agree on a topic where they trigger any marker they send to each other : 

{{< figure src="/images/tutorials/canary/canary-token4.png" alt="a cool tumbnail about pihole" >}} 

Since they agreed earlier not to click on the marker (i.e. the file to download), if someone else does, they have a proof that their conversation is not private.

## Some explanations
---
In the case of the Canary Tokens service, you enter your email address, a side note for your canary and the website generates a marker that you can then place as a link in one of your emails, private messages or even in files (e.g. Word, PDF, etc.).

{{< figure src="/images/tutorials/canary/canary-token1.png" alt="a cool tumbnail about pihole" >}} 

When the marker is triggered, the website will send an alert message directly to your mailbox : 

{{< figure src="/images/tutorials/canary/canary-token3.png" alt="a cool tumbnail about pihole" >}} 

## Dig a little deeper
---
For the more technical folks, you can also use it as a DNS, SQL, or even QR Code marker. 

This way, in case of a lookup in your QR Code or network or if an update, select, insert or delete at a certain place in your database is done, you will also receive an alert in your inbox.

{{< figure src="/images/tutorials/canary/canary-token2.png" alt="a cool tumbnail about pihole" >}} 

Same thing if someone browses your network shares without authorization, with a specially designed directory to alert you if it is accessed from a Windows machine (thanks to the old good booby-trapped desktop.ini trick).

## Make your own !
---
In short, it's not a new concept, but it's great for the most paranoid or those who have suspicions. Of course, the weak point of this trick is the canary URL. If canarytokens.com is blocked by your spy, the alerts will not be triggered.

However, the good news is that you can host the CanaryTokens script on your own server thanks to this [Docker image](https://github.com/thinkst/canarytokens-docker) (or the [repository](https://github.com/thinkst/canarytokens)). This way you will have your own subdomain or domain reserved for your CanaryTokens to surprise more easily any eavesdropper.

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️


 

