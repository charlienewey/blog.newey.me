---
layout: post
title: Making a Mosaic from Random Images
date: 2016-04-03 16:32:33.000000000 +01:00
---

You may have noticed that this blog has been quiet for a while. That's because
I've been writing my undergraduate dissertation (the subject of another
soon-to-be blog post, and hopefully a published paper)! This post is to make up
for my lack of activity on here lately -- so let's get started.

<!-- more -->

If you've seen any recent pop art (or in fact, kept abreast of the arts section
in most newspapers), you'll probably have seen things like this (found
[here][1]);

[1]: http://cornejo-sanchez.deviantart.com/art/Marilyn-Monroe-mosaic-208815986
![A mosaic of Marilyn Monroe made from
emojis](/images/marilyn-monroe-mosaic.jpg)

Or this;

![A mosaic of Barack Obama made out of
toast.](/images/toastbama.jpg)

Yup, that really is a mosaic of Barack Obama's face made out of toast (of
varying degrees of toasted-ness).

Lately, I was thinking about making something similar as a side project, using
a set of random images as mosaic tiles. The idea was to make something -- maybe
to print out a bunch of pictures and arrange them into a poster, for example.
However, the process of finding a large enough and diverse enough set of images
is time-consuming, and figuring out which images should go where is really
labour-intensive. I'm FAR too lazy to do all of that by hand.

Fortunately, this is actually pretty easy to do if you know a bit of Python...
which I do. So then, let's get started. There are two steps to this
mini-project: acquiring and pre-processing a set of images, and then arranging
them into a mosaic.

### Acquiring and pre-processing image tiles

I wasn't sure where to get images from originally -- maybe Wikipedia Commons?
Maybe Flickr? Maybe some public-domain images? Eventually (because of
laziness), I simply settled on using Flickr images. I wrote a short Python
script to search Flickr for photos with a particular set of tags (and of
course, filtered the images by Creative Commons licences), and download them.

Then, I wrote another script to centre, crop, and resize these images into
32x32 pixel tiles. Now that the images are preprocessed into tiles of the same
dimensions, they are ready to be used in the mosaic.

For the sake of prototyping, I went ahead and downloaded some 500-odd Creative
Commons images from Flickr and pre-processed them into tiles. It's a really
small set of images, so I didn't expect it to work very well -- but as you'll
see later, I was pleasantly surprised.


### Arranging into a mosaic

This is actually blindingly straightforward. A simple (plain English) algorithm
for arranging these pre-processed images goes something like this:

* Read in the target image (the image to be made into a mosaic)
* Read in all of the image tiles
* For each image tile
    * Calculate the tile's average colour
    * Store that in a data structure somewhere (we will use this later)
* Create a blank image, with scaled up dimensions - to accommodate the tiles
* For each pixel in the target image
    * Get the pixel's colour in *RGB*
    * Find the image tile with the *closest-matching* colour
    * Paste the corresponding image tile into the blank image we created
      earlier
* Save the output image

#### Finding the best-matching tile

This is dead simple, too. Because it was the easiest thing to do, I simply
averaged each colour channel across the entire image, and made the assumption
that the resulting colour was the *canonical colour* of the entire image.

Finding the best match for an individual pixel in a set of image tiles is
actually quite straightforward, too -- just take some distance measure (I used
[Manhattan distance][2]) between each colour channel between the pixel and the
*average* colour of each tile, and then sort the image tiles by distance. The
tile at the top of the list will be the *best match*. Simples.

[2]: https://en.wikipedia.org/wiki/Taxicab_geometry


### The results

I did a little experimentation to figure out if it worked. Initially, it worked
surprisingly well, but (as I'd guessed would probably happen), a lot of images
got re-used and made the overall effect a bit rubbish. I'll demonstrate this
using a rather fetching picture of President Obama (in keeping with the toasty
tribute earlier).

For reference, here's what the target image looks like:

![President Obama's face](/images/obama-face.png)

My initial attempt worked, but looked a bit like this:

![Crappy first attempt](/images/obama-mosaic.jpg)

Great! It works! ... but so many images are used several times, it sort-of
ruins the effect. OK, so what if... instead of choosing the *closest matching
tile*, I randomly choose one from the *top 5 closest matches*?

![Much better second
attempt](/images/obama-mosaic-attempt-2.jpg)

Much better! That's definitely more interesting. And for the sake of
completeness, let's try another face... what about Michelle Obama?

![Michelle Obama's face as a
mosaic](/images/michelle-mosaic.jpg)

Awesome. What abouuuuuuut (crap, I've run out of Obamas, what next?)... um...
what about my own face? (sorry)

Here's what I look like normally: ![Me!](/images/me.jpg)

And here's what I look like as a mosaic: ![Mosaic
me!](/images/me-mosaic.jpg)

Success! Not bad for a couple of afternoons' work. If you're interested, the
code is available on [my GitHub account][3] (it's *very* sloppily put together,
so don't judge the code quality too harshly ;) ).

[3]: https://github.com/charlienewey/image-mosaic
