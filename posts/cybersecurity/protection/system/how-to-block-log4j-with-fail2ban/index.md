# How to block Log4j with fail2ban


So today, we will see how to detect and immediately ban using fail2ban, all the creeps that try to exploit your servers with log4j.


## First things first
---
If you haven't done so yet, check out [here](https://github.com/cisagov/log4j-affected-db/blob/develop/README.md) the official CISA log4j vulnerability guidance, which includes all the responses from a bunch of software vendors.

## The problem

The problem with the internet is that too many computers, in my opinion, have for the sole purpose to scan and detect all accessible and vulnerable servers. 

This includes to test such servers with old vulnerabilities from 20 years ago, as well as with zero-days that will be known to the public in the next 6 months.

So, whether you're still trying to patch log4j and trying to buy yourself some time to patch all your servers or, at the opposite, you're totally relaxed because your machines are no longer vulnerable (at least for now 🤠), 

the best solution would be to detect and ban any present and future communication with these dubious IP addresses when they are trying to exploit your infrastructure.

## The magic solution

To do this, add the following rule to `/etc/fail2ban/jail.local` : 

```
[log4j-jndi]
maxretry = 1
enabled = true
port = 80,443
logpath = /path/to/your/*access.log
```

Then create the file `/etc/fail2ban/filter.d/log4j-jndi.conf` and put the following regex definition in it :

```
[Definition]
failregex = (?i)^<HOST> .* ".*$.*(7B|{).*(lower:)?.*j.*n.*d.*i.*:.*".*?$
```

And that's it!

For reference, the code gist is [here](https://gist.github.com/jaygooby/3502143639e09bb694e9c0f3c6203949).

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
