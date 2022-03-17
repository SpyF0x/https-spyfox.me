# DNS servers : 1.1.1.1, 8.8.8.8 or 9.9.9.9 ?


You may not be aware of it, but every time you type a URL in the address bar of your browser, it is transmitted in clear text to the DNS server you are using. 


## A bit of background
---

The latter then takes care of resolving the domain name, and tells your machine how to connect to the right server that will distribute the expected data.

But here's the problem : DNS is a 40 years old protocol, and at that time engineers were not too sensitive to privacy issues. So the overall confidentiality of DNS is nearly inexistant. 

In most cases, DNS requests are made in clear text. Thus, an entity located between you and the DNS server has the complete ability to know which websites you are visiting.

However, solutions do exist such as DNS-over-TLS and DNS-over-HTTPS, which allow DNS requests to be encapsulated in encrypted protocols. Unfortunately, they are still not widely implemented.

## Google : 8.8.8.8
---

{{< figure src="/images/privacy/dns/8.8.8.8-google-dns.jpg" alt="a cool Google DNS image" >}} 

Google DNS does its job very well, but of course, all the domain names you resolve via 8.8.4.4 and 8.8.8.8 are added to the long list of personal data collected about you.

## Cloudflare : 1.1.1.1
---

{{< figure src="/images/privacy/dns/1.1.1.1-cloudflare-dns.jpg" alt="a cool Cloudflare DNS image" >}} 

Cloudflare, which already accelerates a large part of websites thanks to its CDN, now offers its own DNS. They commit to not resell the data, and they don't keep the logs beyond 24 hours.

On the one hand, Cloudflare is doing you a favor, but in exchange, you are providing a networking company your browsing data. This is neither good nor bad, but it is important to be aware of it.

## Quad9 : 9.9.9.9
---

{{< figure src="/images/privacy/dns/9.9.9.9-quad9-dns.jpg" alt="a cool Quad9 image" >}} 

Quad9 is run by a non-profit organization, and offers the same level of security as Cloudflare's, with a little bit of intelligence in threat detection to keep you from ending up on malware or phishing websites.

## The best is yours !
---

Anyway, whether you choose 9.9.9.9, 1.1.1.1 or even 8.8.8.8 is up to you, but this is an opportunity to stop collecting your personal data.

You can install your own DNS server on your computer, your phone or your server, which will resolve everything for you. 

It takes 5 minutes with [this tutorial](/tutorials/setup-your-own-dns-server-with-ease/) and you will be fully independant. No more tracking, no more censorship, get full control over your data.

## Make a good action !
---

Hit that CTRL + D to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

