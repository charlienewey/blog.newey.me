---
layout: post
title: Both Text and Binary Logs are Terrible
date: 2015-05-07 14:16:00.000000000 +01:00
---

## (when used badly)

This topic has been rather controversial lately on [Hacker][1] [News][2], and I
thought I'd write a little about it as I thought I could add another viewpoint
to the discussion, particularly about approaching logging from an archival
perspective.

<!-- more -->

OK, so the title of this post is a bit click-baity (and perhaps deliberately
so). For that, I apologise, but *"both text storage and binary storage have
their own use-cases and both are actually pretty good"* isn't so catchy. What
I'm really suggesting is that neither binary logs nor text logs are a perfect
solution to everyone's logging problems - there are situations and use-cases in
which either one is entirely appropriate, and there's nothing wrong with that.

So here is a somewhat topical defence  of the many use cases for text logs. I
won't cover binary storage, as that topic has been talked about very well
[here][3] and [here][4]. I'm open to corrections, discussion and interesting
points of view!

[1]: https://news.ycombinator.com/item?id=9496850
[2]: https://news.ycombinator.com/item?id=9504028
[3]: http://asylum.madhouse-project.org/blog/2015/05/05/grepping-logs-is-terrible/
[4]: http://asylum.madhouse-project.org/blog/2015/05/07/grepping-logs-is-still-terrible/

# Text Logs Are Great

## Longevity

Imagine a situation where you want your log files to be useful for a long time
- say, 10 years or more. You might dismiss this scenario out of hand, but
actually this situation is relevant for a surprising number of people.

For example; I spent some time at the IT department at a car manufacturer a few
years ago, and one of the things I was most amazed by was simply *how long they
kept everything*. Because cars are a safety-critical product (and their
operational lifetime is so long), common sense (and their legal department)
dictates that they must keep as much information about each car for as long as
possible. This includes part numbers, batch numbers, which machine performed
which operation, and so on. Thousands of parts, on thousands of cars, built by
thousands of people and thousands of machines? That's a lot of log data.

Reading logs that are stored in an opaque binary format 20 years after the
format was designed poses a significant problem to anyone trying to extract
meaning or value from the data. This is a problem I have first-hand experience
with at my [current workplace](http://www.ceda.ac.uk/) - we deal with (mostly
binary) scientific data dating back to the 1980s.

Some of the binary formats for the scientific data were designed in a fairly
arbitrary manner and often so poorly specified that some of the data is
essentially unreadable. Because the format specification has been lost to the
sands of time (or it never really existed), there exist large chunks of
scientific data that nobody knows how to read any more.

Comparing this to the plain text data formats that we curate (NASA Ames,
HITRAN, CSV, TSV, JSON, etc), and it becomes abundantly clear that plain text
is easier to read long-term - plain text will always retain its
human-readability, even if its structure is forgotten over time. Binary? Well,
not so much.

My point? You can bet your bottom dollar that ASCII and UTF-8 text formats will
still be readable in 30 years. Whether that's relevant to you and your systems
is another thing entirely.


## Principle of Least Effort

Another situation - you're running a small Linux machine (perhaps as a media
centre, perhaps as a file server, perhaps as a digital photo frame... you get
the idea). You apply some updates to the machine and for some (as yet unknown)
reason, the kernel fails to initialise when you reboot.

Imagine that you now have to boot from a Linux distro on a USB stick to
diagnose and rescue the Linux box. Which logging storage format presents a
larger obstacle to diagnosing the problem; text logs or binary logs?

The answer will most likely be that text logs (even gzipped ones) are easier to
read and will present a lower barrier to entry - and in this case, makes the
problem easier to solve. Even the most barebones Linux distributions come with
the basic line-oriented text manipulation tools like `grep`, `awk`, `sed`, and
so on. You won't have to spend an hour faffing around trying to get extra code
compiled just so that you can decode some binary logs.

Is it worth setting up a centralised logging server just for something so
small? Of course not. So just keep the log files in text - so that in the rare
situation that you need to read them, you can do it with any old Linux distro
and the standard tools.


## Avoiding Lock-In

Most binary log storage formats tend to be linked to a toolset that's unique to
a part of the system or logging infrastructure that uses them.

What I mean by that is: I can't read `journald` logs on a machine that doesn't
have `systemd`, and so on. This is mostly OK - I mean, it's no trouble if the
system you need to diagnose is still running, but what if you can't boot?
You're essentially restricted to using a machine that runs `systemd` to analyse
the log files.

If you want your logs to be *portable* (or to be able to analyse them on
another machine) then your best bet is to stick with plain text.

If storage is an issue, almost every Linux system ships with some form of GZIP
support - which is of course, an excellent way of compressing text.


# Conclusion

If you use either binary log storage or text log storage in the wrong
situation, the chances are that you will find your choice of logging storage
cumbersome, irritating and opaque - in other words, your logs will **suck**.

Choosing the best storage format depends on a number of factors:

* The longevity of your system (1 year? 40 years?)
* The long-term value of your log files
* The size of your system (1 node? 10,000 nodes?)
* The need for centralised logging
* The quantity of your logging outputs (100kb/day? 100gb/day?)
* The need for indexability and searchability

There is no single *right* answer for doing logging properly, because it really
all depends on your system's requirements - and there's no sense suggesting
that there's a one-size-fits-all solution, because there isn't one.
