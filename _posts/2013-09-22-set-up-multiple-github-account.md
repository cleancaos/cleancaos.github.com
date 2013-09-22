---
layout: post
title: "Set Up Multiple Github Accounts"
description: ""
category: set-up
tags: [git, github, future-reference]
---
{% include JB/setup %}

**Assumptions:** Working on Mac OSX, working with ssh

**Prerequisites:** At least 2 github account already created 

<br/>

###Post Overview:###

1. Create SSH keys
2. Upload RSA keys on github
3. Create .config file for RSA keys
4. Set up user.name and user.mail

---
#### Sample variables for this post #### 

johndoe = Mac OSX User

john@home.com = email 1st github account

john@work.com = email 2nd github account

---
**Step 1: Create SSH keys**

Within the terminal digit:
`ssh-keygen -t rsa -C "john.doe@home.com"`

When the promt asks where to save type:
`/Users/johndoe/.ssh/id_rsa_home`

Do not use any passphare.

<br/>

Do the same for the other key.
Digit `ssh-keygen -t rsa -C "john.doe@work.com"`, save in `/Users/johndoe/.ssh/id_rsa_work` and do not use passphase.

<br/>

Because the keys have a unique name, we need to tell SSH about them.
Within the Terminal, type:
`ssh-add /Users/johndoe/.ssh/id_rsa_home` 

If successful, you’ll see a response of “Identity Added.”
Do the same for the other key, typing `ssh-add /Users/johndoe/.ssh/id_rsa_work`

<br/>

**Step 2: Upload RSA keys on github**

Now, we have to copy the public key content and paste it on github.

On terminal:

`pbcopy < ~/.ssh/id_rsa_home.pub`

On [github](http://www.github.com) go to "Account Settings"

![alt text][github-settings]

On the left column choose "SSH Keys".
![alt text][ssh-key]

Add SSH Key. Give it any title you want and paste the content.
![alt text][add-ssh-key]


[github-settings]: /img/2013-09-22/github-settings.jpg "Github settings"
[ssh-key]: /img/2013-09-22/ssh-key.jpg "SSH Key"
[add-ssh-key]: /img/2013-09-22/add-ssh-key.jpg "Add SSh Key"

<br/>

Do the same for the other account.

Copy the public key `pbcopy < ~/.ssh/id_rsa_work.pub` and paste it on your second github account.

<br/>

**Step 3: Create .config file for RSA keys**

In /Users/johndoe/.ssh/ directory, create a file called ".config"
For each account you'll have 4 lines. You can choose the first of these lines: the "Host" (for further personalization see the references).

{% highlight c %}
Host jhome
  HostName github.com
  User git
  IdentityFile /Users/johndoe/.ssh/id_rsa_home

Host jwork
  HostName github.com
  User git
  IdentityFile /Users/johndoe/.ssh/id_rsa_work
{% endhighlight %}

<br/>

**Step 4: Set up user.name and user.email**

It is important that in the global git configurations it is _not_ configured any user name and email.
You're gonna set up only local user name and email for every git repository, so you can always choose which account to use.

Let's distinguish 2 cases: 
- You create a local git repository
- You clone a remote git repository

<br/>
####Create local####
When you create a local git repository with `git init` the folder .git is created within the project directory. Inside .git directory there's the "config" file and initially no user name and email are set.

So if you want to push git doesn't know which github account to use. You tell it by configuring the local user name and email. Let's say we're gonna use the work account, you type (assuming your current terminal directory is the project directory, feel free to use the `cd` command to make it sure):

`git config --local user.name "John Doe"`

`git config --local user.email "john@work.com"`

Now, if you open the "config" in the .git folder (which is a hidden folder, but you already know it) you'll see this new lines:

{% highlight bash %}
[user]
	name = John Doe
	email = john@work.com
{% endhighlight %}

<br/>
#### Cloning remote####
On the other hand when you clone a remote repo you're gonna use the host set in the ssh ".config" file. To do that we have to use the ssh link of a repository provided by github.

Let's say we wanna clone the Jekyll Bootstrap repository [https://github.com/plusjade/jekyll-bootstrap](https://github.com/plusjade/jekyll-bootstrap)

On the page we'll copy the ssh link ![alt text](/img/2013-09-22/ssh-link.jpg)

In the directory project (that you already created) you digit:
`git clone git@jhome:plusjade/jekyll-bootstrap.git`


[_Notice that we used the "jhome" host._]

Then you set the url origin to your own repository on github and finally you set-up the local user name and email like before, in that way git knows which account to use and its credentials.



---
## References ##
- [My git setup](http://kevinoncode.blogspot.it/2011/11/my-git-setup.html)
- [Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys)
- [How to Work with GitHub and Multiple Accounts](http://net.tutsplus.com/tutorials/tools-and-tips/how-to-work-with-github-and-multiple-accounts/)
- [multiple ssh private keys](http://www.karan.org/blog/index.php/2009/08/25/multiple-ssh-private-keys)
- [How to Use Multiple GIT Accounts](http://sandarenu.blogspot.com/2011/09/how-to-use-multiple-git-accounts.html)

<br/>
---
---


<br/>