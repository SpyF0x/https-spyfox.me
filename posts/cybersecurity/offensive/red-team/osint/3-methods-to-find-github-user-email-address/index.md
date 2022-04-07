# OSINT : 3 methods to find GitHub user email address


## A bit of context 
---

I'm going to list three methods, from the most stealthy and less effective to the less stealthy and most effective to gather an email address using GitHub.

There is no particular attack conducted against GitHub users. It is only a matter of iteratively browsing all the information that is publicly available. 

## First method : Profile README !
---

Since 2016, GitHub allows users to use a Markdown file named "README.MD" to write details about themselves, such as their skills or interests. Here's an example of this feature on my GitHub account : 

{{< figure src="/images/osint/github-mail/Screenshot-SpyF0x-Github.png" alt="web pentest" >}} 

The problem here, is that many people also go as far as writing their name, nationality or even take inventory of all their social network accounts including their email address at the same time. As you can guess, all these infos simplify a LOT the data collection process.

I admit that simply taking a look at a GitHub profile is not technical at all, but hey ! It's still open source intelligence ! 

Plus, it gives a very small footprint because GitHub sees you as a normal visitor browsing a normal user profile. The only downside however is that it will only work with users that have : 

1. Created a profile README.
2. Put in it their email address. 

You are consequently highly dependent on what the user is willing to share with the Internet. However, when I see how many people pay nearly no attention about their privacy online, it just makes me want to do more on my website to make more people aware. It is mainly for this reason that I started writing articles in the first place.

{{< figure src="https://media.giphy.com/media/4SXW1rU9loG1HIXXg2/giphy.gif" alt="web pentest" >}} 

## Second method : Git commits metadata
---

I'm always impressed to see what you can gather when it comes to analyze metadata. 

As a reminder, metadata is all the data used to describe another data.

{{< admonition example "Example" true >}}

When you send a text message to your friend Bob, an example of the metadata you generate may be : 
- Who you are writing to (Bob)
- At what time (hh:ss)
- How often (often)
- Where you sent the message from (your GPS location)
- Where your friend Bob was when he received it (his GPS location)
- ...
{{< /admonition >}}

That's surely a lot of info ! Now, if someone cross-references other metadata like your Google searches, your browsing history or all the GSM antennas you pass by, that person could know absolutely everything of your conversation without ever knowing the content itself. In short, metadata can tell us a LOT and should be as protected as the data it describes. 

In the case of GitHub, we can use the metadata of a commit to extract the email address of the user who actually made the commit :

### Step 1 : Choose a non-forked repository

In this screenshot of my public repositories, you can see that "umami" has been forked. As I'm not a major contributor of umami, we will most likely never find a single commit from myself if we choose that repo.

That's why we actually need a non-forked repo. In the screenshot below, any other repo will do the trick :

{{< figure src="/images/osint/github-mail/Screenshot-SpyFox-Github-repos.png" alt="web pentest" >}} 

### Step 2 : Locate a user commit 

Once you have chosen a repository, go to the list of commits. Our goal is to find one or more commits created by the user we are looking for. To illustrate this, I've chosen my "Effective Binder" repo : 

{{< figure src="/images/osint/github-mail/Screenshot-SpyFox-Github-effective-binder.png" alt="web pentest" >}} 

As you can see, I was the only one committing, so any commit will do the trick. Just click on the commit ID on the right side to get some details. 

### Step 3 : Display the raw commit 

Once done, we are facing this kind of page : 

{{< figure src="/images/osint/github-mail/github-commit.png" alt="web pentest" >}} 

In fact, we don't care that much to this page because we need to modify the URL by adding ".patch" at the end. From : 
{{< figure src="/images/osint/github-mail/url-spyfox.png" alt="web pentest" >}} 

to : 
{{< figure src="/images/osint/github-mail/url-spyfox-patch.png" alt="web pentest" >}} 

Wich leads us to this page : 

{{< figure src="/images/osint/github-mail/commit-patch.png" alt="web pentest" >}} 

### Step 4 : extract email address 

The email address is located in the second line of that page : 

{{< figure src="/images/osint/github-mail/github-mail.png" alt="web pentest" >}} 

{{< admonition info "Info" true >}}

As you can see, my email address has been kind of scrambled as it is actually a "no reply" github email address. If you are interested to do the same, have a look at this post ([here](/posts/cybersecurity/offensive/red-team/osint/hide-your-email-address-from-github/))
{{< /admonition >}}

## Third method : Some scripting
---

This method is more of an automation of the second method than a totally different one. I present it anyway because I know it could be very useful for red teams to get a way to automate the collection of email addresses in case of a campaign against an entire organization, for example.

Let me introduce you "Zen" by s0md3v : 

{{< figure src="/images/osint/github-mail/zen-mail-github.png" alt="web pentest" >}} 

### Usage 

If you only have a username, just type : 

`python zen.py username`

If you have the URL of a GitHub repo, it's like this : 

`python zen.py https://github.com/username/repository`

And if you are targeting a specific organization, this command is what you need : 

`python zen.py organization --org`

There is a rate limit based on the number of requests you send to GitHub per minute. For example, if you are not authenticated, you will be limited to 60 requests per hour compared to 6,000 requests per hour if you are authenticated.

Zen also integrates an email breach check (haveibeenpwned lookup) with : 

`python zen.py username --breach`

## To conclude 
---
With the tricks described in this post, you will be able to get the mail addresses of most of the user profiles on GitHub.

If you don't want to leave your email address lying around on GitHub, take a look at this article, which shows you how to protect your email address from such techniques.

## Because I'm not a rocket scientist, the references  
---
- https://github.com/s0md3v/Zen
- https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme
- https://www.thereach.io/blog/articles/how-to-find-github-user-email-addresses

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

