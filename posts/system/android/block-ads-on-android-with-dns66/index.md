# Block ads on Android with dns66/dns66


How to block ads on your Android smartphone ? Here's a simple and yet effective tutorial on how to block ads on your Android smartphone.


Don't worry, this does not require any technical background at all !


## # First things first

Advertising could be a good idea, but for a few years now, it's become a real mess : between tracking, intrusive ads or even videos that start by themselves... 

We, the consumers, can no longer suffer this tyranny without saying stop !

Tyranny because Android as well as IOS, being developped by companies financed by advertising (and also by our private life) will never let you a nice and clean stop button to block the ads. 

At best Google, like Microsoft, Apple or Facebook will only let you a small switch to disable targeted advertising (meaning disable targeted ads to put simple ads instead)

## # Solution : dns66/dns66

dns66/dns66 is an Android application that uses a VPN interface on your phone to filter the content that passes through the Wi-Fi or cellular network. Thus, via hosts files, you can block domains referenced as offering advertising (Google Ads for example).

The configuration is not complicated because everything is managed automatically. You just have to start the application and activate lists of domains to exclude. These lists are updated daily.

{{< figure src="/images/tutorials/dns66/dns66.png" alt="a cool tumbnail about pihole" >}} 

## # Did you say _"VPN"_ ?

Don't confuse a VPN server which will make all your web traffic transit via a server by masking your IP and VPN interface which uses this way only to filter the local content, your IP address doesn't change as well as your download / upload speed.

The result is immediate: ads are gone once for all ! You can verify dns66/dns66 is doing its job thanks to the little key in the status bar on the top right, wich is how your Android notify you about an active VPN interface.

{{< figure src="/images/tutorials/dns66/dns661.png" alt="a cool tumbnail about pihole" >}} 

## # What about battery life ?

Unlike other solutions, I have not seen any negative impact on either my battery usage or page display speed. It's not faster, either. However, what a relief for the eyes !

I invite you to test it on your smartphone to make an opinion for yourself. As it is against Google's Terms Of Service by default, you will only find it on F-Droid as usual !

[{{< figure src="/images/tutorials/fdroid.png" alt="a cool tumbnail about pihole" >}}](https://f-droid.org/app/org.jak_linux.dns66/dns66)

[Here](https://github.com/julian-klode/dns66/dns66)'s the official GitHub repository for reference. 

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
