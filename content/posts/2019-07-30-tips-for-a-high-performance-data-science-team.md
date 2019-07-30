---
title: Tips for a high-performance data science team
date: 2019-05-31
meta: true
draft: false
---

Over the last couple of years, I've been working as a data scientist in a small
aviation technology company based in the UK. We're a fairly new team, and we've
come across some difficulties over the years. This blog post is a bunch of
anecdotes and tips about the problems that I've come across while leading my
team. I guess it's a distillation of my (limited) experience --- but it's only one
person's account so take it with a pinch of salt. :)


## Beware of white elephants

Sometimes, you'll find that the more senior members of staff in your
organisation think they know more about data science than they do. This can
take many forms - ranging from suggesting "project ideas" that are a bad fit
for a team of skilled data scientists (e.g. ), or projects with success
criteria that are difficult or possible to measure.

Some of this is related to the way that data science is perceived in your
organisation, and the best way to fix this problem is with regular and clear
communication with the rest of the business ([I go into a bit more detail about
this later on](#share-your-knowledge)). However, I'm not talking about that
here --- I'm talking about making sure that your project is worthwhile.

The main thing to remember is that you are part of a *data-driven* team.
Everything that you do --- ranging from the time you spend working on different
projects, to the number of models you create, to the impact of your data
science projects --- should be measured and quantified. Your role is to deliver
*measurable* business value. This isn't always easy to do, but you can make it
easier by measuring as many relevant data points as you can.

Let's say that your CEO is pushing hard for your team to develop a
recommendation system for features on your company's website. Before launching
into any preparatory work, you should ask the CEO challenging questions to
extract as much information about the project as possible. I've found that
sometimes projects proposed by management haven't taken these things into
account --- and will flounder when you ask challenging questions. For example,
focusing on any of the below topics will usually help you (and the manager)
understand if the project is worth doing.

1. Measurement
    * What is the expected outcome of this project?
        * An increase in return customers?
        * Customers spending longer interacting with the website?
        * Customers paying extra for additional features?
    * Do we have enough monitoring in place to be able to measure the outcome?
    * Do we know that developing these features will *actually* help?
2. Maintenance
    * Is this a one-off project, or a long-lasting service?
    * Are the data science team going to need to do significant maintenance?
    * Who is going to be responsible for the project long-term?
4. Value
    * What is the expected return on investment?
    * Does the benefit from the project outweigh the cost of using the DS team?
    * How is the project expected to affect the outcomes?

These questions are useful when critiquing project ideas that come from within
your own team as well. It's too easy to get excited about working on a
particular project when it might not actually have that much business value.


## Sell your team within the company and share your knowledge

<a name='share-your-knowledge'></a>

In the beginning, a lot of the non-data-science staff treated the data science
team as something a bit separate --- and a bit magic. Data science and machine
learning kind of have this weird aura of wizardry about them (at least, they
did at my company), and it's a fairly harmful perception because people might
feel intimidated asking you questions or suggesting things to you.

The best way to tackle this problem is to communicate to everyone about your
data science work! You want to raise awareness throughout the business of what
you've done. Once other staff get an idea of what you're capable of and the
sort of things that you can do, they will begin to approach *you* with
questions.

There is a caveat to this, however --- make sure that your communication is
accessible to the layperson. Keep it clear, concise, and straightforward.
Share the fundamentals --- nobody outside of your team cares about which
hyperparameters you decided to tune on your model --- focus on what problem you
solved, how you measured the impact, and maybe (maybe!) which algorithm you
used. If you're using slides or some other kind of visual aid, use lots of
pictures, keep it low on text and maths, and try to make it interesting.

Keep a knowledge repository (a blog-type space to hold information about your
experiments and projects). Not only will this serve as a great training
resource for new data scientists joining your team, it will also make leaving
the company much easier (your handover will already be half-completed).
[AirBnB have released a really nice, polished tool for exactly this sort of
content.](https://github.com/airbnb/knowledge-repo).

Run workshops and training sessions for non-technical employees. Even if this
involves a five-minute "what we've been up to" at the beginning of the monthly
company meeting (or even sending out a link to your latest post in the
knowledge repository) can help raise the profile of your team and teach people
about what you're capable of. Finally, if everyone has a clear intuition of
what your projects are doing, you're enabling people to ask high-quality
questions and provide genuinely constructive criticism. Don't underestimate how
valuable this can be!


## Close your feedback loops before you start working

One common problem in fledgling data science teams is incomplete or delayed
feedback --- a situation that I refer to as an "open feedback loop". Open
feedback loops can significantly frustrate your efforts in trying to develop a
meaningful data science product --- either by slowing your ability to refine
your product, or by preventing you from evaluating the quality of your results.

Here's a situation that I encountered at my own workplace; I wanted to try and
reduce our customers' expenditure on new tyres for their fleets --- aircraft
tyres can cost thousands of dollars apiece --- so I spent some time researching
a project that would help airlines extend their tyres' lifetimes.

This included investigating the different factors that affect tyre wear --- how
different runway surface types wear tyres differently, how longer runways
enable faster approaches (and therefore increase tyre wear), how different tyre
compounds wear at different rates, and so on. I also spent some time designing
some experiments and building a plan for a regression model that would enable
me to estimate the accumulated wear on a set of tyres.

After all of this work, there was one thing I had failed to take into account;
even though I had data on approaches, runway surface types, aircraft weights,
and so on... I didn't have access to any information from our customers about
*when they had to change their tyres*. I didn't think this would be a problem
--- after all, I just had to ask the customers about when their tyres were
changed. However, when discussing the idea with our commercial team, it
turned out that there was some legalese buried in our customer contracts that
would have made it difficult for us to acquire that data.

Long story short? Before you do too much preparatory work and launch headfirst
into a project, make sure that it's possible to close your feedback loops
first.


## Question everything about your data

This is fairly self-explanatory. Real-world data is troublesome!

Data quality is going to be an issue, no matter the project you work on.
Whether it's due to faulty sensors, poorly-trained humans, a poor user
interface, or any one of a myriad of sources of error... your data is going to
be imperfect. Learning to cope with imperfect or incomplete data is as much an
art as it is a science, and is almost always more complex than just shoving the
median or modal value into the blank spaces. It usually requires a bit more
understanding of the system that created the data.

One of the most effective ways of addressing these problems is to try and
understand the full "chain of custody" of your data. In the context of
aviation, this means figuring out things like why the engine vibration readings
between different types of aircraft are so different (vibration sensors are
located in different parts of the engine on different aircraft), and much more.
You'll often need to talk to people that are responsible for different parts of
the processing chain (e.g. sometimes you'll need to talk to pilots, sometimes
engineers, sometimes aviation data specialists, and so on).

Learning the full provenance of your data can be a challenge but it rewards you
with a deep domain-level understanding of the data. If you're able to
use your intuition to guesstimate the importance of a feature before
incorporating it into a model, it can save you lots of time (and will help you
produce something worthwhile much more quickly).

Oh, and one more thing. Be wary of any data that's come into contact with a
human. Humans are monstrously error-prone.


## Provenance and reproducibility are king and queen

So this kind of follows on from the last section. It's key to make sure that
you and your team know what's happening to your data at *each* stage of your
data science pipeline! Having access to this information means that you'll
easily be able to reproduce a successful result when you happen across one.

There are a few tricks that make this easier;

1. Always version-control your code with Git or something similar
2. Keep an informal experiment log about things you've tried
3. Separate each stage of your processing code into its own script --- it makes
   a pipeline much easier to manage.

There are a plethora of tools to help with this --- there's `git` for your
code, and more sophisticated tools for versioning your data (like `dvc`).
These tools will keep your code and your data consistent --- which means that
you should be able to run the code in your repository at *any given time* and
know that the output is exactly the same as the last time you ran it. Being
able to reproduce your experiments quickly and easily is the mark of an
accomplished data scientist.


## Quantify your impact as much as you can

When you start a new data science team, you initially expect that your team's
value will speak for itself. Hopefully it will, but it never hurt anybody to
keep track of the proof. Hope is a dangerous tactic, and data science
departments are expensive --- you should be ready to show (at any given moment)
that your team is generating business value.

For example, if you're trying to develop a machine learning system with the
goal of improving customer engagement, how do you measure the effect you're
having?

* Do you use time and motion analytics on your website?
* Do you measure an increase in customer value using sales figures?
* Do you track the level of engagement with your social media posts?

If you don't already measure these quantities (or whichever other quantities
are more relevant to you), you can't possibly measure the effects of any of
your data science solutions. This not only makes it difficult for you to know
that you've accomplished your goal, but it also makes it impossible to prove to
the business that you've generated something of any value at all.

Even if you *do* measure relevant metrics, they don't always tell you anything
useful. Lots of these sort of metrics have a particularly slow feedback loop
(i.e. there's a long wait between taking an action and seeing a result).
Ideally you want to be *immediately* able to check the effect of any changes
you make! If it takes too long to get feedback, you're running the risk of
wasting time working on the wrong thing.

One of the most important things you can do is design your workflow in a way
that helps you get *prompt* and *accurate* feedback. This workflow will vary
from project to project --- it may mean creating a Jupyter notebook that
generates a one-page performance report for you to give to management, or it
may mean that you need to hold regular standups to report on your team's
progress... it doesn't matter. The important thing is that it should be quick
and easy to see results.


## Involve as many people as possible

Data scientists tend to be fairly creative folk in general. This is great, and
we can often come up with valuable ideas --- at least, we *think* they're
valuable. Sometimes we misjudge that and end up chasing projects that capture
our attention. It's too easy to think of an exciting project and run away with
it before actually determining whether it's useful!

This can happen in organisations where there are degrees of separation between
your "customer" (or product owner), and the data science team. For example, in
my previous company, customer relationships were structured like this;

1. Customer
2. Customer Relationship Manager (CRM)
3. Data Scientist

The degree of separation here is enough to derail successful communication
between you and your customer. Being separated from your customer is likely to
cause you *both* headaches in the long-term --- all too often everyone ends up
playing a game of Chinese Whispers --- and this results in the customer getting
a shoddy product and the CRM (and your team!) getting poor feedback.

This can be made more difficult when you're trying to explain parts of a
complex system to a non-technical audience. Having a non-expert CRM try and
explain a difficult technical concept to the customer can really fuel confusion
--- and there can be long-lasting negative effects from a misunderstanding.

The easiest way to prevent this is to make sure that your team is directly
involved in communicating with customers. After all, the data science team are
the experts --- and sometimes your customers need to hear from experts ---
particularly if they're good at communicating difficult ideas concisely and
simply. If possible, this should be done with the involvement of your CRMs!
Your CRMs will have a better long-term rapport with your customers, and will
probably have a better understanding of their needs, as well as a higher
quality of insight into their business.

Keeping on top of customer communication can stop difficult situations arising
further down the line --- and involving more of your company's staff in your
projects will raise the profile of the data science team. Win-win!


## Wrapping up

Well, if you're still here... I commend you. I originally meant to write a few
short paragraphs about some situations that I've come across during the last
couple of years, but this has spiralled into something much bigger. I hope that
you've found this post useful --- maybe not all of these tips will apply to
your team, but hopefully there's a nugget of information in there that you can
take away and get more out of your data science team.
