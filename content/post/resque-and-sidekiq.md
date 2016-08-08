+++
date = "2016-08-08T07:53:33-07:00"
title = "Resque and Sidekiq as of August 2016"

+++

## Document status
Work in progress.

## Motivation
Some months ago, at an interview I got asked this:

> What would you use [resque](https://github.com/resque/resque) or [sidekiq](https://github.com/mperham/sidekiq) and why?

My answer was something along the lines of:

- Sidekiq consumes less memory, so that sounds better.
- Both Resque and Sidekiq are stable and widely used in production.
- I've used Resque in a 3 applications, two of them are still running and processing tons of background jobs.
- Before making my choice I would need to know the kind of background processing that we would do. I want to know if the jobs would be [IO bound or CPU bound](http://stackoverflow.com/questions/868568/what-do-the-terms-cpu-bound-and-i-o-bound-mean).

I want this writing to be:

1. A helper to uncover a better answer.
2. A mental model and decision framework to help me make a choice the next time.

## What is background processing?

## What the internet says about it?

## PROs and CONs

## How your problem impacts the choice

## Mental model and decision frameworks
- https://hbr.org/2007/11/a-leaders-framework-for-decision-making
- https://mentalmodelsblog.wordpress.com/
http://mentalmodels.princeton.edu/about/what-are-mental-models/
