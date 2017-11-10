---
layout: post
title: The Root of All Evil
date: 2015-06-14 17:27:44.000000000 +01:00
---

Logging into a Unix machine as *root* is almost always a bad idea. I've come to
this conclusion through several years of making silly mistakes and breaking
important things on my own machines... usually mistakes that end up with me
reinstalling my operating system. After destroying my system for the *nth* time
because I was doing things as *root*, I decided to write this blog post -
mostly as a reminder to myself not to be an idiot, but perhaps this will be
useful to others, too.

<!-- more -->

### Multi-user madness

If you allow *root* logins on a multi-user system (and freely give out access
to the *root* account), you remove all accountability and essentially defeat
the purpose of the system's authentication logs. Allowing multiple users
administrative access to a machine means that any commands executed show up in
the system logs as being executed by... *root*, and not the actual user.

This is a bad idea. Take the following hypothetical situation:

> A development team of several people all have *root* access to a production
> machine. A team member has recently been fired or made redundant and harbours
> a grudge. The system administrator has forgotten to remove their SSH key or
> removed the wrong key by mistake, leaving the aggrieved ex-employee with root
> access to a machine. Something *magically* goes wrong with the production
> system shortly afterwards - all of their databases are dropped (oh no!) and
> all of the backups are nowhere to be found.
>
> The system administrators *know* that the damage was most likely caused by
> the ex-employee, but the logs just point to the root user - and as several
> people were able to log in as root, nobody could prove that the ex-employee
> had anything to do with it.

Oops.

### Human stupidity beats artificial intelligence

Almost everybody that's ever installed or configured a Unix system or that has
regularly used Linux themselves will have some experience of catastrophically
breaking their system without meaning to. Whether through inexperience,
incompetence, inattention, or just plain old bad luck, most Unix users will
have a story along the lines of:

> "I was formatting a USB thumb drive when I accidentally typed in `sdb1`
> instead of`sda1` and for some reason, my computer wouldn't boot any more"

Or perhaps:

> "I was trying to `rm -rf /etc/nginx/*` but I accidentally left a space
> between `/etc/nginx` and `/*` and it deleted everything on my filesystem
> partition"

Or maybe it'll be:

> "I remember the time when I deleted some random file called `libc.so` to save
> disk space, and I was really surprised when everything stopped working"

Accidents happen, typos happen, and people do stupid things without thinking -
it's part of human nature. There are so many Unix horror stories ([like
this][1], [or these][2]) that you really don't need to read much until you
realise how easy it is to slip up and destroy something important by accident -
don't tempt fate by executing all of your commands with superuser privileges.

[1]: https://web.archive.org/web/20140130224140/http://web.mit.edu/bayleyw/www/notes/yak
[2]: http://www.yak.net/carmen/unix_horror.html


### Computers make very fast, very accurate mistakes

Actually, this is kind of an important point. Software bugs are pretty common
in computer-land, and accidentally wiping your entire hard drive is as easy as
[trying to run a game in Steam][3] (which contained a misconfigured shell
script that ran `rm -rf /` on your main OS disk when it was configured
incorrectly). This isn't a unique occurrence, either - in fact, [Red Hat
recently had a similar issue with Squid][4] that caused several testers' hard
drives to be wiped, too.

So, what's my point? Well, just like everyone else, programmers are fallible
too - and sometimes mistakes make their way into production code. Save yourself
the hassle of accidentally breaking everything and just run applications as a
standard user with restricted permissions. Otherwise you run the risk of
deleting important stuff, overwriting vital files, corrupting filesystems,
accidentally deleting shared libraries, or a hundred-and-one other things you
can do to break your operating system.

[3]: https://github.com/valvesoftware/steam-for-linux/issues/3671
[4]: https://bugzilla.redhat.com/show_bug.cgi?id=1202858


### People are nasty, too

By running normal userland applications or services as root, you're giving
yourself a higher chance of getting stung by malware - for example, if you have
ever copied-and-pasted shell commands from the internet, then you might've
[accidentally executed a malicious command hidden inside the web page][5].
Running standard userland applications with elevated privileges (like a web
browser) basically gives malicious software or exploits a free pass to do
whatever they please to your computer, without even trying!

[5]: http://thejh.net/misc/website-terminal-copy-paste


### SSHhhhhhh...

Allowing root logins over SSH is a bad idea because it essentially means that
if an SSH key is compromised then a malicious user can directly log in with
*root access* on the target machine... which isn't something that's usually a
positive experience.

If you disallow *root* logins over SSH and instead provide power users with
*sudo* privileges, it adds an extra layer of authentication between a malicious
user and superuser access by requiring a password to authenticate to superuser.
Requiring this extra level of authentication might lessen the consequences of
having a compromised user account, and it has an added bonus of increasing
accountability - `sudo` will add entries in the system's auth log whenever
someone tries to use `sudo` to escalate to superuser.


### Long story short?

As the old adage goes; With great power comes great responsibility. Using the
root account all the time is widely regarded as bad practice, it's insecure for
a number of reasons, and it makes the consequences of making a mistake more
severe than they need to be.

Treat it with respect, use it sparingly, and give your power users *sudo*
access instead.
