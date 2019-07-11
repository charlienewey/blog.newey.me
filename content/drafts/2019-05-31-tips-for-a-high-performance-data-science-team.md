---
title: Building a high-performance data science team
date: 2019-05-31
meta: true
draft: true
---

Over the last couple of years, I've been working as a data scientist in a small
aviation technology company based in the UK. In this team, we've come across
the "cold-start problem". This is something that many new data science teams
come across, and there are lots of pitfalls that a new team can fall into along
the way.


## Beware of rabbit-hole projects

Sometimes, you'll find that the more senior members of staff in your
organisation think they know more about data science than they do. This will
typically involve suggesting "project ideas" that they think your team might be
able to do, but often these projects aren't a good fit for a data science team
--- and even more often, their success criteria are difficult (or impossible)
to measure.

You are part of a *data-driven* team. Everything should be measured and
quantified. Your role is to deliver business value --- *Measurable*
business value.

Let's say that your CEO is pushing hard for your team to develop a
recommendation system for features on your company's website.


*put something in here to flesh out the story*


## Sell your team and share your knowledge

* Shout about your work! You want to raise awareness throughout the business of
  what you've done. Once other staff get an idea of what you're capable off,
  they will begin to approach *you* with questions.
* Keep a knowledge repository (a blog-type space to hold stories).
* Run workshops and training sessions for non-technical employees.


## Question everything about your data

* Data quality is going to be an issue wherever you go.
* Learn the full provenance of your data (as much as you can, anyway).
* Identify what processing steps your data goes through before it reaches you.
* Learn where humans come into contact with it. Humans mess data up. Always.


## Close your feedback loops

One commonly-encountered problem in fledgling data science teams is incomplete
feedback --- a so-called "open feedback loop". This is definitely a situation
I'm familiar with, and it can significantly frustrate your efforts in trying to
develop data science product.

As an example, here's a situation that I encountered at my own workplace. I
wanted to try and reduce our customers' expenditure on new tyres for their
fleets (aircraft tyres can cost thousands of dollars apiece), so I spent some
time researching a data science project that would help airlines extend their
tyres' lifetimes.

I spent time researching how I might approach the project, and the different
factors that affect tyre wear --- how different runway surface types wear tyres
differently, how longer runways enable faster approaches (and therefore
increase tyre wear), how different tyre compounds wear at different rates, and
so on. I also spent some time doing feature engineering and building a plan for
a regression model that would enable me to estimate the accumulated wear on a
set of tyres.

After all of this work, there was one thing I'd failed to take into account;
even though I had data on approaches, runway surface types, aircraft weights,
and so on... I couldn't get access to any information from our customers about
*when they had to change their tyres*!


## Quantify everything

When you start a new data science team, you initially hope that your team's
value will speak for itself. Hopefully it will, but it never hurt anybody to
keep track of the differences they've made to the business. This is a dangerous
tactic --- data science departments are expensive! You have to be able to
continually show that your team is generating business value.

For example, if you're trying to develop a machine learning system with the
goal of improving customer engagement... how do you measure the effect you're
having?

* Do you use time and motion analytics on your website?
* Do you measure an increase in customer LTV using sales figures?
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
you make! If your feedback loop is too slow, you're running the risk of wasting
your time working on the wrong thing.

Therefore, one of the most important things to do is design your workflow in
such a way that you can get *prompt* and *accurate* feedback. Whether it means
writing a Jupyter notebook that generates a one-page performance report for you
to give to management, or whether you start holding regular standups to report
on your team's progress... it doesn't matter. The important thing is that it
should be quick and easy to see results.


## Involve everyone

Data scientists tend to be fairly creative folk in general. This is great, and
we can often come up with valuable ideas --- at least, we *think* they're
valuable. It's too easy to think of an exciting project and run away with it
--- before actually determining whether it's useful.

This can happen in organisations where there are degrees of separation between
your "customer" (or product owner), and the data science team. For example, in
my previous company, my customer relationships were structured like this;

1. Customer
2. Analyst/Customer Relationship Manager
3. Data scientist

Notice the degree of separation? This is enough to derail successful
communication between you and your customer. Being separated from your customer
is likely to cause you headaches in the long-term --- for example, if the CRM
misinterprets customer feedback (or misquotes it), then everyone ends up
playing a game of Chinese Whispers --- the customer gets a shoddy product and
the CRM gets poor feedback.

This can also be made more difficult when you're trying to explain parts of a
complex system to a non-technical individual. Having a non-expert CRM try and
explain a difficult technical concept to the customer just causes confusion ---
and more often than not there will be long-lasting negative effects from such a
misunderstanding.

The easiest way to circumvent this is to make sure that your team is directly
involved in communicating with customers. After all, the data science team are
the experts --- and sometimes your customers need to hear from experts. If
possible, this should be done with the involvement of your CRMs! Set up a Skype
call or a meeting with your customers, but make sure that the CRM is present
too. Your CRMs will have a better long-term rapport with your customers, and
will have a better understanding of their needs.


## Make everything reproducible

* Version control data like you do with your code. You do use `git`, right?
* Script each processing stage. Keep those scripts under version control.
* Version control your entire data pipeline! Reproducibility is king.


## Log everything
