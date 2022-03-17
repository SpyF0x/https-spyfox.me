# Facebook - What can we learn from Facebook's business disruption


Every downtime is an opportunity to learn, even for companies like Facebook Inc. that seem unwilling to learn from mistakes in other areas.

## A little bit of context
---
Facebook Inc. experienced a severe outage in its main operations on Monday, Oct. 4. The massive downtime was difficult for those responsible for building and maintaining its technology and applications to cope with. While downtime is not new to Facebook, this outage is certainly one that will go down in the company's history books.

Facebook Inc. released a brief statement on the evening of Oct. 4, primarily to refute conspiracy theories that had been spreading on social media. More details about what caused the business to go down were later released on Oct. 5.

A blog post published by the company essentially confirmed what was already known, as detailed by Cloudflare: Facebook Inc. somehow managed to block access from the outside Internet to servers running Facebook, Instagram, WhatsApp and other properties, while performing routine maintenance.

Facebook operates a vast network of facilities, including their own big data centers but also smaller facilities called "points of presence" scattered around the world, to collect inbound traffic and direct it to its final destination through Facebook's internal network. It's a sort of custom Content Delivery Network (CDN) to increase availability, speed up load times and load balance all the traffic.

## The technical background
---
Servers and network equipment are prone to failure for a variety of reasons, and checking for any failures on the network is part of the engineering staff's daily routine. But on the morning of Oct. 4, the routine check was somehow executed and served as an order to withdraw all Facebook connections from its backbone network.

In a post it published, the company said the auditing tool that was supposed to detect potentially catastrophic errors in configuration changes, failed because a bug in the auditing tool prevented it from terminating the issued commands.

The infrastructure choices that Facebook Inc. operates compound the problem, and decisions made long ago regarding its internal facilities make recovering from this error much more difficult than it would be for other companies.

In contrast to Facebook, which relies almost entirely on its own infrastructure and custom services to meet nearly all of its operational needs, other technology companies of the same size and resources are using infrastructure provided by third-party providers, at least in part, to meet their needs.

This includes DNS servers, which run in those smaller access point facilities. These servers tell Facebook's data centers where incoming requests for its content are coming from and provide a path for browsers requesting "facebook.com" to the computer at that destination (e.g. if european user it would be X.X.X.1, if american user it would be X.X.X.2, etc.)

Facebook's DNS servers are designed to inform inbound requests to "facebook.com" and, if they detect a problem with that path, they avoid the particular path to the data center, as delays can lead to a poor user experience. Under normal circumstances, there are many more working paths than faulty paths, and it is easy to find a quick detour.

## The incident
---
When all of these paths are gone, DNS servers that operate in other ways didn't know where Facebook's servers are, forcing them to return error messages to phones and browsers.

{{< figure src="/images/security/facebook-down/facebook-down.png" alt="web pentest" >}} 


Making things even more difficult, Facebook's internal communications and disaster recovery tools rely on connections to the facilities that house these DNS servers.

Everything described so far happened in the span of about two minutes on the morning of Oct. 4. The important thing is that Facebook needed to recover quickly from this world-scale error, and that recovery was more difficult than ever. And for some reason, Facebook's out-of-band connection to its servers (the normal backup plan when the main network goes down) also failed. This meant that physical access to its data center facilities was required to fix the problem.

While Facebook didn't actually need to modify its server framework to fix the problem, but dealing with the server failure in question took more time than one might think.

## What can we learn 
---
Every downtime is a learning opportunity, even for a company like Facebook. Here are three lessons that can be learned from this incident : 

- Prepare for the worst. Companies need to have plans for a complete loss of computing resources or network connectivity, not just a data center or cloud computing area that goes down but the worst scenario.
- Use the services of multiple service providers. While it is highly unlikely that the Internet as a whole will go down, it is worthwhile to employ the services of multiple service providers.
- Check priorities. Operations on the scale of Facebook's cannot be implemented without the use of extensive automation, which means that code auditing tools (such as the one that failed to prevent this outage) need extra attention.

## Make a good action !
---

Hit that CTRL + D to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
