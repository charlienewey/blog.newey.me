---
layout: post
title: My experience working at a cargo cult startup
date: 2017-07-13 00:22:55.000000000 +01:00
---

> "In the South Seas there is a Cargo Cult of people. During the war they saw
> airplanes land with lots of good materials, and they want the same thing to
> happen now. So they've arranged to make things like runways, to put fires
> along the sides of the runways, to make a wooden hut for a man to sit in,
> with two wooden pieces on his head like headphones and bars of bamboo
> sticking out like antennas...

<!-- more -->

> ...he's the controller --- and they wait for the airplanes to land. They're
> doing everything right. The form is perfect. It looks exactly the way it
> looked before. But it doesn't work. No airplanes land. So I call these things
> Cargo Cult Science, because they follow all the apparent precepts and forms
> of scientific investigation, but they're missing something essential, because
> the planes don't land." [Richard Feynman, "Cargo Cult
> Science"](http://calteches.library.caltech.edu/51/2/CargoCult.htm)[^1]

In early 2016, I applied for an internship as a data scientist at a machine
learning/cognitive science startup (which shall remain nameless) as something
to do over the summer. During my initial research into the company (and even up
to the interview), first impressions were good, and the project looked
exciting. They wanted to build a revolutionary hiring and recruitment platform
using machine learning to match candidates with job openings, and I leapt at
the chance. Most enthusiastic CS undergraduates would have bitten someone's
hand off for the opportunity to working on a project like this. I started the
summer looking forward to getting stuck in, and prepared for my first day.

## The first alarm bells

On day one, the first thing I did was sit down with the "CTO" (an odd title for
the *one* techy guy in a 4-person company) and begin an induction - what I
thought would be a fairly high-level overview of the project I was meant to be
doing. I was hoping I'd be introduced to their core software, their
infrastructure, their data and the structure of their data, any analyses they
might have performed already, and so on.

Not quite. I sat there for ~45 minutes while he explained `for` loops, `if`
statements, and much more besides. I had recently graduated with my degree in
computer science by this stage, and (supposedly) he had an MSc in CS. I said
that I was comfortable with all of this stuff within the first couple of
minutes, but he blustered on regardless. After half an hour, I started to
notice that I had alarm bells ringing in my head - and the realisation set in
that maybe these guys weren't quite what they said they were.


## ~~Data science~~

The second thing I learned is that the founders wanted us to build a "data
science and machine learning pipeline" to basically estimate an individual's
compatibility with a job description. I assumed that they'd already have some
plans in place for this - whether that be ideas for machine learning
algorithms, semantic analysis, feature extraction from job descriptions and
CVs, etc. Nothing. This wasn't too bad, I thought to myself - I'll just look at
their data and start building some of the basics during this internship.
Imagine my surprise then, when I discovered that they had no data at all.
Literally none. No CVs, no job descriptions, no dummy data entered into their
website from beta testers - nothing. This company had been going for 2-3 years
at this point - I wondered to myself what they had been doing all this time.

Further to this, it turned out that their "data scientist rockstar" was a guy
with a business degree who had apparently done an online course on Hadoop. I
don't really think I need to say much else to be honest, but to be clear; a
single Coursera course doth not a data scientist make. Most of the interns
(myself included) ended up teaching him rudimentary technical stuff (like
*programming*), and I'm not sure how (or why) he was hired as a data scientist
in the first place.


## Fuzzy logic

They didn't know what they wanted to build. Even if they knew what they wanted
to build, they didn't know how to build it. Even if they knew how to build it,
they lacked the technical skill to actually implement it. It was eminently
clear from the start that none of the founders had any idea what they were
doing - and it seems they thought that they could just bring in a few students
to scribble equations on whiteboards and get all of their "algorithm
development" out of the way in two clear months.

A common phrase that we heard in the office was "just make sure that our main
algorithm is finished by week 5 so that we can put it into production". In this
case, they were talking about writing an algorithm that would match job
candidates to job specifications - a task, which, by the way, has stumped some
of the world's best machine learning experts and cognitive scientists,
including several very large (and very highly skilled) teams at LinkedIn and
other companies. If companies like LinkedIn haven't cracked that problem yet,
I'd be very surprised if 4 interns could do it in 5 weeks. Nothing's
impossible, I suppose.


## Unusual work ethic

Founders came in on the "executive train" at 11am. This would've been expected
in a Fortune 500 global company, or even a small 30-odd employee company that
had successfully been supporting itself for several years. This startup was
none of those things - it employed a grand total of *two* people besides the
founders, and constantly turning up at midday looked pretty weird. Did they
expect the interns to do everything? Did they work a lot from home? They never
mentioned this to anybody or explained anything - it just looked like they
didn't care. More to the point, they didn't seem to do a lot when they were
actually *in* the office anyway. For the 3 hours a day that they were actually
*in* the office, they didn't seem to do a lot besides chatting, "networking"
and pissing about on VentureBeat and Crunchbase.


## Exit stage left

The permanent staff seemed to frequently mention their "exit". In startup
parlance, an "exit" is what happens when the company builds a product that
works well enough to float them on the public stock market (i.e., an IPO) or
that grabs the attention of another, bigger business - which then buys the
startup out. This, of course, doesn't work (and won't happen) if you don't have
a product to sell. Nobody seemed to grasp this.


## All the gear but no idea

Everything in the office looked new and shiny, and there was a mini pool table
sitting in the corner next to a stack of board games. At first glance, it
looked like a pretty inviting place for work, but after a month, I learned that
they were only there as a joke or as something to look good for potential
investors; they were certainly never actually used. The founders seemed to have
a thing for spending money on other stuff that they didn't need, too - the CTO
asked me to spec up and price a £5000 server for "running machine learning
models" and "hosting the website". Never mind that they could do all of that on
Amazon Web Services for under £50/month, they *had no machine learning models
to run in the first place*. Why would you buy expensive processing hardware
for... hosting a website? Even running ML models on cloud services like AWS
makes sense, as they can scale with elastic loads - which is much cheaper for
running the occasional ML model.

More to the point - the CTO didn't know what specification of server machine to
buy, he didn't know how to assemble it when it arrived, he didn't know how to
install an operating system on it, and he certainly didn't know how to build
machine learning models. The first three responsibilities fell squarely into my
lap, despite the fact that I was only at the company for two months. The latter
part (building ML models) was the sole responsibility of myself and the other
interns. Oh, and he wanted the server to have "a good GPU for gaming". Right.


## Stagnation and slow death

I'm publishing this blog post about 12 months after I finished the placement.
Out of curiosity, I've been checking their website every so often to see what's
new. In that year, their website... hasn't changed, actually. Not even a
little. In fact, according to [archive.org](https://archive.org), it's *exactly
the same* as it was when I started. It turns out that my overall impression of
the company (that the founders were wholesale bullshit merchants who wouldn't
know an ML algorithm if it sat on them) was correct. As far as I can tell,
they've been sort of stagnating ever since - the company Twitter account still
periodically tweets LinkedIn articles about "recruitment" and "startups" (but
is getting less and less engagement as time goes on). Generally, they appear to
be starting the long, slow slide towards insolvency - and good riddance.


## Final thoughts

While I might be coming across as pretty negative about this company - and be
under no illusion, *I am* - I gained a lot by working there. Sure, I didn't get
a lot out of it in terms of technical skill, but it could be argued that I
learned something much more valuable; a finely-tuned bullshit detector.
Spending time at this company let me learn some of the indicators that I was
working for a cargo cult startup - and because this was during an internship, I
was able to do it in an environment with no financial risk. My placement there
was not something I'd wish to repeat - and nor would I wish it on anyone else -
but I definitely came away having learned something.


## TL;DR: big red flags

* **Vapourware**. Founders trying to sell a product that doesn't exist yet,
  with skills they don't have, that relies upon data that they don't have, to
  an audience that never wanted it in the first place.
* **Founders buying expensive equipment that they don't understand**, can't set
  up, and can't possibly hope to make use of. Why waste vital bootstrap capital
  on expensive kit that nobody has the technical capability to use?
* **Employees unrealistically focusing on acqui-hires and IPOs** as if they
  were real possibilities, when they've barely managed to build a website in
  two years.
* **Founders who don't show up for work**. Most successful founders put in 50+
  hours per week, and there's no exception when it comes to "revolutionising
  the world of recruitment". If it was possible to build a world-beating
  machine learning startup in 15 hours a week, then many more people would be
  doing it.
* **Extensive use of Silicon-Valley-esque bullshit-speak**. If a startup's main
  website reads like a game of buzzword bingo, it probably is. Whether it's an
  ego trip for the founders or just a shiny website designed to trick gullible
  investors into parting with their cash, beware of startups that appear to be
  overselling themselves.

[^1]: With kind reference to [Leo Polovets][polovets] for the Feynman quote and
      for inspiring this blog post

[polovets]: https://codingvc.com/startup-cargo-cults-what-they-are-and-how-to-avoid-them
