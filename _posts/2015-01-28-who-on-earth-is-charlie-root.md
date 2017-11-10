---
layout: post
title: Who on Earth is Charlie Root?
date: 2015-01-28 22:03:33.000000000 +00:00
---

As I've mentioned before on this blog, I've recently moved my website and some
other services over to a FreeBSD server - and when you set up FreeBSD, it runs
several automated checks as `cron` or `periodic` jobs. I've configured FreeBSD
to email me various system logs on a daily basis, so that I get an idea of
what's happening on my server every day.

<!-- more -->

Most of these emails are sent by the `root` user on the system - this is no
coincidence, as the automated checks contain privileged information about
available updates, security problems with user accounts UID changes on files,
and so on. These emails are forwarded to my personal email account using a
Postfix alias. When these emails come through to me, oddly enough they appear
to come from a user called "Charlie Root". Originally, I just thought I'd
misconfigured my server (my name is Charlie, so I thought I'd entered that as
the `root` user's first name by accident) - but upon [further
Googling](http://freebsd.1045724.n5.nabble.com/Happy-Birthday-Charlie-Root-Charlie-Whom-btw-td4246280.html)
it turns out that "Charlie Root" is a common name in BSD for the `root` user.

The only reference that I can find to a Charlie Root online is this [20th
century baseball player](http://en.wikipedia.org/wiki/Charlie_Root)
coincidentally named Charlie Henry Root - or C. H. Root (`chroot`)! Charlie
Root died in 1970 (around when the [initial BSD
distribution](http://en.wikipedia.org/wiki/Berkeley_Software_Distribution#1BSD_.28PDP-11.29)
began development), so I guess this might be related. Then again, perhaps not -
and the reason behind "Charlie Root" is lost to the sands of time.
