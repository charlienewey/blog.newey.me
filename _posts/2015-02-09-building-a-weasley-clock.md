---
layout: post
title: Building a Weasley Clock
date: 2015-02-09 23:32:02.000000000 +00:00
---

#### TL;DR

I gutted an old clock, re-painted it, replaced the clock face, and replaced the
insides with a some techy stuff that moves the clock's hand to show my
location. For those of you who don't want to read this entire post, I've
included a video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-_c85UT44cc" frameborder="0" allowfullscreen></iframe>

<!-- more -->

Check out [the source on GitHub](https://github.com/charlienewey/weasley)!

### What is a Weasley clock, and why make one?

As a child, I really enjoyed reading the Harry Potter books, and I always
thought that the Weasley family's clock sounded like a really interesting
device. For those who don't know about this clock - it's essentially a magical
clock that has a hand for each member of the family, and a number of
"locations" around the clock face. The hands point to each family member's
location, thus telling you where they all are.

The one that the Weasley family has in the film looks like this: ![The Weasley
family's clock](/images/weasley.jpg)

### How does it work?

There are several parts that make up the clock;

* To control the servo that moves the hand, I used a [Spark
  Core](https://www.spark.io/) - a small, wireless-enabled embedded computing
  device that's compatible with [Arduino](https://arduino.cc/) code.
* To track my location,  I used the [OpenPaths](http://archive.is/SuZCi)
  location API - which I linked with my mobile phone.
* To tie it all togetherI wrote some Python code to sit on my web server that
  regularly fetches my OpenPaths data, checks proximity using a [Great Circle
  Distance](http://en.wikipedia.org/wiki/Great-circle_distance) function, and
  sends cloud commands to the Spark Core to update the position of the clock
  hand.

### Building the Clock

Small embedded computing systems have got massively cheaper in the last few
years, and I have a bit of free time on my hands every so often - so I hit upon
the idea of building one a couple of years ago.

It took me a while to get the parts together and they've been sitting around in
my spare room for... well, probably about a year. Until last weekend, that is -
when I decided to break out the chalk paints, buy a Spark Core, and finally put
it all together.

Writing [the code](https://github.com/charlienewey/weasley) was actually the
quickest and easiest part! The longest part of the process was actually getting
the clock, sanding it down, painting it and putting it all together. I thought
it might be interesting to include some photos of the build, so I've included
some below.

Deciding on the locations was **hard** - cutting out the stencils was a
nightmare because there were so many potential mistakes, so I decided on only
three locations to put on the clock face:

* Home
* Travelling
* Work

I might add more later on if I feel like destroying any more stencils...

#### Pre-Sanding

![Clock Pre-Sanding](/images/clock-1.jpg)

#### Painting

![Clock after Painting the Body](/images/clock-2.jpg)

#### Stencilling the Clock Face

![Clock Part-Way through Stencilling](/images/clock-3.jpg)

#### The Finished Body

![The Finished Clock Body](/images/clock-4.jpg)

#### Wiring

![Wiring the Servo Up](/images/clock-5.jpg)

#### Sticking it all Together

![Putting the Motor in the Clock](/images/clock-6.jpg)

#### The Finished Product

I've included a short YouTube video of the clock cycling through locations at
the top of this post!

Enjoy :-)
