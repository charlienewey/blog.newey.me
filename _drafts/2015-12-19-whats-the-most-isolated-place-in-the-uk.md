---
layout: post
title: What's the most isolated place in the UK?
---

I've always enjoyed exploring great open landscapes - both on foot and in cars -
that's part of the reason I studied in Aberystwyth, after all. One of the things
I frequently noticed is that the most awe-inspiring landscapes - with the
wildest, most impressive views - tend to be in areas with little human
habitation. This is hardly a profound revelation, and yet it holds true for most
of the best landscapes in the UK. Some examples of this phenomenon immediately
spring to mind; Snowdonia, the Cambrian mountains, the Lake District, the Peak
District, the Grampians, and others - all of these gorgeous landscapes are
located in the remotest and least populated corners of the country.

This got me wondering; how easy would it be to find the *most isolated* part of
the UK - and were the landscapes as likely to be lush and wild as I suspected?
Of course, there's only one way to find out - to look at every part of the
country. Short of embarking upon a very long road trip, this didn't seem
feasible - so I had to cheat and use a computer.

Luckily, the UK is a very well-mapped part of the world - and there are several
freely-available datasets from the Ordnance Survey containing information about
the exact locations of postcodes, settlements (i.e. cities, towns, villages,
etc), and much more besides. The two best datasets for this task are the [OS
Code-Point Open][1] dataset (comprising of some 1.7 million postcodes and their
exact coordinates) and the [OS Open Names][2] dataset - comprising of the
coordinates of every major settlement in the country.

Between these two datasets, it should be possible to build up a comprehensive
picture of the most deserted areas in the UK - and from there, figure out if the
scenery is impressive enough to warrant visiting. There are a couple of
different ways of approaching this problem - and I learned a surprising amount
from tackling this problem in different ways.

The first approach I tried was to simply find the most isolated postcode within
the United Kingdom - this was an easy enough job, as all I needed to do was
download the Ordnance Survey's "Code-Point Open" dataset and build a KD-tree
from the points. Once I'd built this, it was a case of computing the distance
between each postcode and it's *nearest neighbour* - then the postcode with the
greatest distance from their nearest neighbour would be the most isolated.

However, this isn't the full story, as I'm seeking the most isolated *point* in
the UK... so actually it makes more sense to take a different approach.


[1]: https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-open.html
[2]: https://www.ordnancesurvey.co.uk/business-and-government/products/os-open-names.html
