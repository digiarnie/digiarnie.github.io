---
layout: post
title: Multiple GitHub Accounts on One Machine
excerpt: "Have multiple GitHub accounts?  Make sure you're committing with the correct GitHub account."
modified: 2014-09-23
tags: [github]
comments: true
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

If you're like me and have multiple GitHub accounts (work and personal) then you will probably want to make sure the correct GitHub user is being used for each specific repository.  Here is the set up I've used to make sure my work repositories are committed using my work GitHub account and my personal repositories are committed using my personal GitHub account.

First let's create two SSH key pairs: one for work and one for personal.  When *ssh-keygen* prompts for the file location, make sure you give two different file names.  Here I have simply suffixed the file name with .work and .personal respectively.

{% highlight bash %}
$ ssh-keygen -t rsa -C user@myworkemail.com
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): /home/user/.ssh/id_rsa.work

$ ssh-keygen -t rsa -C user@mypersonalemail.com
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): /home/user/.ssh/id_rsa.personal
{% endhighlight %}

Now add both keys to your SSH authentication agent.

{% highlight bash %}
$ ssh-add id_rsa.work
$ ssh-add id_rsa.personal
{% endhighlight %}

Copy the contents of your work public key and log into your *work* GitHub account and add the work public key to your SSH Keys section.

{% highlight bash %}
# This will output the contents of your work public key
$ cat id_rsa.work.pub
{% endhighlight %}

Do the last step again to associate your *personal* public key with your *personal* GitHub account.

{% highlight bash %}
# This will output the contents of your personal public key
$ cat id_rsa.personal.pub
{% endhighlight %}

Now you need to create a *config* file in ```~/.ssh``` to identify which set of keys to use for which situation.

{% highlight bash %}
$ cd ~/.ssh
$ vim config
{% endhighlight %}

In the *config* file, add the following contents.  

This will map your personal key with the host ```personal``` and the work key with the host ```work```.  Both point back to the host *github.com*.

{% highlight %}
Host personal
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa.personal

Host work
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa.work
{% endhighlight %}

If you wanted, you could make either the ```personal``` or ```work``` Host to be called github.com so that you don't have to change the remote URL (as you will see further on) for both personal and work repositories.

You can verify that the keys and SSH configuration have been set up correctly by testing the SSH connection:

{% highlight bash %}
$ ssh -T personal
Hi personal-user! You've successfully authenticated, but GitHub does not provide shell access.

$ ssh -T work
Hi work-user! You've successfully authenticated, but GitHub does not provide shell access.
{% endhighlight %}

To make sure GitHub links your identity correctly in its system, make sure the repository you are dealing with has the correct *user.email* in the Git configuration.  For example, let's say globally I have a *user.email* set to *user@myworkemail.com* but I am in a personal repository.  If I were to push changes, GitHub will show my work GitHub account as being the committer.  To ensure that my *personal* GitHub account is shown up as the committer, set the *user.email* to the personal e-mail address.

{% highlight bash %}
$ cd ~/personalRepository
$ git config user.email "user@mypersonalemail.com"
{% endhighlight %}

When you are ready to push changes to GitHub, just make sure that your URL for origin on remote is set using the correct host (in our case, either *personal* or *work*).  The following is an example of pushing changes to a personal repository using the personal keys.  **Note:** The token after the @ symbol used to say github.com.  We are now replacing it here with ```personal``` to make sure we end up using the correct identity file.

{% highlight bash %}
$ git remote set-url origin git@personal:personal-user/personalRepository.git
{% endhighlight %}

Obviously you'll have to do the same for work repositories by using the work host.

{% highlight bash %}
$ git remote set-url origin git@work:work-user/workRepository.git
{% endhighlight %}