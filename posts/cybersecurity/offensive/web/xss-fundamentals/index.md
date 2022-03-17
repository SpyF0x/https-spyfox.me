# XSS - Fundamentals


You have certainly already heard about XSS. But what is really a Cross Site Scripting (XSS), how can a hacker exploit this kind of vulnerability and for what purpose ?


## A little bit of context
---
We regularly hear about Cross Site Scripting. Depending on your level, you know that it is either has something to do with JavaScript or, if you are keen on computer security, you could even state that XSS is a security flaw allowing a user of the site to inject JavaScript code into a web page.

Concretely, if you post something like : 
```
<script>alert('XSS');</script>
```
in a forum and when your comment is displayed, you see a pop-up containing "XSS", congratulations, you have just spoted an XSS.

## What are the risks?

You might think that inserting an alert message isn't terribly dangerous, but if you can inject that, you can inject other scripts that are more malicious. It is not necessary to be able to inject any particular special character in order to attack. If you can inject alert(1) then you can inject any arbitrary script using JS. 

Friends administrators, don't worry. This kind of vulnerability cannot be exploited to gain access to your server (though, we'll see about that later). The targets of hackers who exploit this kind of vulnerability are the users of your app / website.

JavaScript is executed on the client side. It is therefore not possible to execute a script directly on the server using only XSS.
### Concretely, a hacker would be able to :

- Redecorate with some defacement. 
- Use your account if you are authenticated to post messages or perform any other action that you could normally do.
- Redirect you to an authentication page that looks exactly like the authentication page of the site you were on, in order to steal your credentials.
- Spread a malicious source code to all your contacts if the flaw exists on a social network.
- Use your computer to launch DDoS attack or mine some bitcoins

In short, anything that can be done with JavaScript...

## Reflected XSS

In a reflected XSS attack, the attack is in the request itself (frequently the URL) and the vulnerability occurs when the server inserts the attack in the response. The victim triggers the attack by browsing to a malicious URL created by the attacker

`http://www.acme[.]com/?title=<SCRIPT>document.location='http://evil[.]com/'</SCRIPT>`

For instance, accessing this URL will simply redirect to the site http://evil[.]com which may for example launch a Java applet exploiting a bug to install a virus on your computer, before redirecting you back to the legitimate site http://acme[.]com.

{{< figure src="/images/security/xss-fundamentals/xss-fundamentals3.png" alt="a cool image" >}}
*A example on an insecured webapp vulnerable to URL parameter*

## Stored XSS 

In a stored XSS attack, the attacker stores the attack in the application (e.g., in a snippet) and the victim triggers the attack by browsing to a page on the server that renders the attack, by not properly escaping or sanitizing the stored data. 

Note that the victim does not even need to explicitly click on the malicious link. Suppose the attacker owns evil[.]com, and creates a page with an invisible `<img>` or `<iframe>` pointing to the malicious link; if the victim visits evil[.]com, the attack will silently be activated. 

{{< figure src="/images/security/xss-fundamentals/xss-fundamentals5.jpg" alt="a cool image" >}}
*The vast majority of website defacements are made by XSS*

## Attacking a server using an XSS vulnerability

What a catchy title!

Many of you could tell me that it is impossible... Of course, it is not possible to use an XSS flaw to directly hack a server.
But nothing is impossible in hacking, you just have to get around the obstacles. Let's imagine a scenario to achieve that!

What we know about our target :

- There is a management interface using a session on the server.
- The session identifier is kept in the cookies.
- And of course, there's an XSS

The goal of this attack is to do a classic session-hijacking.

{{< figure src="/images/security/xss-fundamentals/xss-fundamentals6.png" alt="a cool image" >}}

### Methodology : 

1. We setup a web server (Python SimpleHTTPServer is a good one)
2. We create a JavaScript to load an image (or an iframe) from our webserver, and we pass a `document.cookie` into the URL call.
3. We insert this script in the page, and we wait for the administrator to see the page.
4. Once the page loaded on the admin side, we get the user cookie through the precedent URL call.
5. Once we have forged the cookie on our browser, all we have to do is to connect to the admin account (no need to identify ourselves)!
6. If the administrator disconnects, we will also be disconnected, so we must take advantage of this short moment to maintain access.

{{< figure src="/images/security/xss-fundamentals/xss-fundamentals7.png" alt="a cool image" >}}


If the management interface allows it, we can take advantage of it to :

- Get the e-mail addresses of all users
- Modify pages of the site to integrate JavaScript code in order to spread the attack
- Infect the server with a virus
- Tamper logs
- Modify pages templates to retrieve server names, database identifiers...
- Delete user accounts
- Delete pages (delete the site?) / Defacements
- Etc.

## How to prevent XSS ? 

 XSS is a difficult beast. On one hand, a fix to an XSS vulnerability is usually trivial and involves applying the correct sanitizing function to user input when it's displayed in a certain context. On the other hand, if history is any indication, this is extremely difficult to get right. US-CERT reports dozens of publicly disclosed XSS vulnerabilities involving so many companies.

Though there is no magic defense to getting rid of XSS vulnerabilities, here are some steps you should take to prevent these types of bugs from popping up in your products:

- First, make sure you understand the problem.
- Properly use modern JS frameworks (READ THE DOCS !!!)
- Use an Auto-Escaping Template System
- Implement Content Security Policy
- Employ good testing practices with respect to XSS.
- Stay informed :)

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
