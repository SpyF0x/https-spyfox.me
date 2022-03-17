# How to enable encrypted DNS (DNS-over-HTTPS) in Firefox


As explained in a [previous article](/privacy/how-to-choose-dns-servers-1.1.1.1-8.8.8.8-or-9.9.9.9/), here is a one of the plethora solution to improve your privacy and your security online dedicated to the Firefox browser.


So you'll have to get your hands dirty, but I know you like that.
Don't worry, it's as easy as pie !

## How does it work \? 
---
The DNS protocol works by translating the domain name of any website you visit each day, sending by sending a query to your DNS server. Same principle for encrypted DNS except that rather than sending in plain text, the query is encrypted via HTTPS.

In this way, the DoH protocol hides DNS queries within normal HTTPS traffic so that third-party (ISP, Hacker, CIA, NSA...) don't know which DNS queries are being sent by users. This makes it impossible to determine which sites are being visited.

{{< figure src="/images/privacy/doh/dns-traffic-over-tls-https.svg" alt="a cool picture" >}} 

Moreover, the protocol works at the application level, meaning that the applications can integrate a whole list of compatible servers to which to send requests automatically (e.g. round robin). 

This list is prioritized over the default DNS settings advertised by your router. In most cases, the latter are defined by your Internet service provider and can't be changed.

Applications that support DoH can therefore bypass local ISP filters quite effectively and access content that may be blocked by government. Which is why DoH is currently being popular. 

It's one step closer to improved privacy and security for users. Considering how widespread mass surveillance is becoming, it's always good to take it!

In Firefox, there are currently 2 ways to enable DNS via HTTPS, but first, make sure you are up-to-date and have at least version 62 of the browser.

## Method 1: Via Firefox settings (easiest)
---
1. Type about:preferences directly into your URL bar.
2. In the General section (the default page), scroll down to Network Settings and click on Settings.

{{< figure src="/images/privacy/doh/dns-traffic-over-tls-https1.webp" alt="a cool picture" >}} 

3. In the window that will open, you will have to check the option "Enable DNS via HTTPS" then select your DNS server. By default, it will be the Cloudflare server with which Mozilla has a partnership, but you can also customize the choice if necessary. Here is a complete [list](https://github.com/curl/curl/wiki/DNS-over-HTTPS) of DoH providers.

{{< figure src="/images/privacy/doh/dns-traffic-over-tls-https2.webp" alt="a cool picture" >}} 

## Method 2: via the Firefox configuration file (advanced users)
---
1. Type about:config in your URL bar and press "Enter". This will take you to the Firefox control panel, where you will need to change 3 things.
2. The first parameter to look for is the one that handles DoH support : `network.trr.mode`. 

{{< figure src="/images/privacy/doh/dns-traffic-over-tls-https3.webp" alt="a cool picture" >}} 

You have to set its value to 2 or 3 depending on your preference. To change the value, just right click -> change or double-click on the line. Here are the other possible values : 

- 0 - Default value in standard Firefox installations (currently is 5, which means DoH is disabled)
- 1 - DoH is enabled, but Firefox picks if it uses DoH or regular DNS based on which returns faster query responses
- 2 - DoH is enabled, and regular DNS works as a backup
- 3 - DoH is enabled, and regular DNS is disabled 
- 5 - DoH is disabled

3. The second parameter is `network.trr.uri`. This is the DoH server URL to query, by default it will be https://mozilla.cloudflare-dns.com/dns-query, but as seen above, other URLs exist.

{{< figure src="/images/privacy/doh/dns-traffic-over-tls-https4.webp" alt="a cool picture" >}} 

Whether you use the first method or the other one, everything should work straight away. If it doesn't, restart the browser.

You'll enjoy a slightly more secure surfing experience.

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

