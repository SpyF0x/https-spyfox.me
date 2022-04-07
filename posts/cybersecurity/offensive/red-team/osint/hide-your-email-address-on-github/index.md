# OSINT : Hide your email address on GitHub


## A bit of context 
---

As a follow-up to this post [(here)](/posts/cybersecurity/offensive/red-team/osint/3-methods-to-find-github-user-email-address-copy/), where I show three techniques to extract the email address of a user on GitHub, I'm posting here an antidote to such techniques. In a nutshell : How to protect your email address on GitHub.

## Why ?
---

Your email is the preferred way for hackers to communicate with you. The days of pixelated logos and bad spelling are long gone. 

Now it's legitimate emails that look like phishing attempts.

But even without counting all the phishing risks you're exposing yourself and your organization to, another way to harm you is through scams:

{{< admonition example "Example" true >}}

If a scammer has your email address, he or she can easily gather other information about you, such as your name, where you live, your children's school, where you work, or even on your co-workers.

This scammer can then call you and use these facts to "prove" that he or she is bona fide, and thus attempt to gain access to other information to impersonate you.
{{< /admonition >}}

Keep in mind that sharing does not always mean caring. You would be smart to never leave out online such critical information as your email address at the mercy of any person or bot that gets it. 

## How ?
---

### Step 1 : Get your no-reply address 

First, go to your GitHub account email settings [(here)](https://github.com/settings/emails) and make sure these two checkboxes are ticked : 

{{< figure src="/images/osint/github-mail-protect/github-email-settings.png" alt="web pentest" >}} 

{{< admonition warning "Warning" true >}}

If, like me, you have created you created your GitHub account after July 18, 2017, your no-reply email address is a seven-digit ID number and your username in the form of ID+username@users.noreply.github.com. 

However, if you created your account prior to July 18, 2017, your no-reply is simply username@users.noreply.github.com. 
{{< /admonition >}}

### Step 2 : Modify your git config

Whether you are using GNU/Linux, macOS or Windows, open a terminal and type : 

`$ git config --global user.email "here-goes-your-no-reply-github-address"`

Then verify with : 

`$ git config --global user.email`

If everything's good, you should get your no reply address and not your personal one.

### Step 3 : Rewrite history 

Here's a script I've found on SO that only applies the change to commits with a matching email address. This way, the whole repository is not overwritten only with your email address. This script uses rebase --root (since git 1.7) to write from the beginning of your history. 

{{< admonition danger "Danger" true >}}
Take all necessary precautions (backups, snapshots, tests, etc.) before running this script ! I am in no way responsible in case of a data loss !!!
{{< /admonition >}}

```bash
function reauthor_all {
  if [[ "$#" -eq 0 ]]; then
    echo "Incorrect usage, no email given, usage is: $FUNCNAME <email>" 1>&2
    return 1
  fi

  local old_email="$1"

  # Based on
  # SO: https://stackoverflow.com/a/34863275/9238801

  local new_email="$(git config --get user.email)"
  local new_name="$(git config --get user.name)"

  # get each commit's email address ( https://stackoverflow.com/a/58635589/9238801 )
  # I've broken this up into two statements and concatenated
  # because I want to delay evaluation

  local command='[[ "$(git log -1 --pretty=format:'%ae')" =='
  command+=" '$old_email' ]] && git commit --amend --author '$new_name <$new_email>' --no-edit || true"


  git rebase -i --root -x "$command"
}

reauthor_all "old_email@example.com"
```

{{< admonition tip "Tip" true >}}
The best way to do this, is on GNU/Linux or macOS. If you only have Windows for it, you can still run this script using the WSL if you already have it installed (and if not, it's [here](https://docs.microsoft.com/en-us/windows/wsl/install) to know how to install it).
{{< /admonition >}}

Create a file named change-email.sh : 

`$ touch change-email.sh`

Copy the whole script to this file and set the execution permissions : 

`$ chmod +x change-email.sh`

And execute it inside any repo you want to modify : 

`[spyfox@compooter target-repo]$ ./home/spyfox/Desktop/script/change-email.sh`

You'll need to force push to overwrite the remote repository : 

`$ git push --force`

 
## Conclusion
---

The different techniques I have presented throughout this post [(here)](/posts/cybersecurity/offensive/red-team/osint/3-methods-to-find-github-user-email-address/) will allow you to hide your email address on GitHub. On the other hand, they won't help if you leave your precious email address lying around. On your README.MD profile page, for example. 

That's why, in addition to these techniques, I invite you to have a look at this post where I list three main techniques to extract an email address from GitHub. Computer security is like an equation, you have to be able to make the solution work both ways to make sure it's working ! 🤠


## Because I'm not a rocket scientist, the references  

- https://stackoverflow.com/questions/34850831/change-git-email-for-previous-commits (the rewrite history script from Matthew Strasiotto 👍)
- https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-user-account/managing-email-preferences/setting-your-commit-email-address/

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

