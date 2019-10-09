+++
date = "2019-07-15T00:00:00-07:00"
title = "How to do more when nobody asks or care"
draft = true

+++

# How to do more when nobody asks or care


## Disclaimer
This is an egocentric talk about me, enjoy your lunch.

1. Has anyone had tasks created by the product or managers where they asks us to refactor pieces or code?
2. Have your managers discussed with you the quality of the code?
3. When you look at code is it nice or ugly?
4. Have you found yourself bored?


## About being ask to refactor
1. Nobody needs to ask me to refactor I feel this is something that is my responsibility.

## About discussing the quality of code
1. How can you objectively discuss code?
2. How do you become objective about code?
3. How many projects have people worked at that helped them figure out what good/bad/nice/ugly is?
4. How do you call bullshit when people "talks about code quality?

## What happens with me?
- I've being surrounded by developers that seemed to have amazing skills, they seemed like artists tom me, I wanted to be like them.
- It wasn't easy to have access to the developers
- I did have access to their creations.
- Code seemed so puzzling, like black magic, full of unknowns and possibilities.
- My curiosity was increased when people would say things such as:
  - This is ugly, it sucks
- I would look at it try to understand why it was bad
- My curiosity increased more when people wouldn't want to touch certain pieces of code.
- People would say it is not worth touching it.
- Then that is me the more I'm told you can't do it the more I want to do it.
- I used to think that I was doing things for the benefit of the business or someone else.
- Deep inside I know I wanna delve into the dungeons of puzzles, I guess I have issues.
- Still one wonders:
  - That is scary, I don't wanna touch that code, I don't wanna go there.
  - How do I safely walk inside?

## Demystify the monster
- Things are less scary when you know what comes ahead.
- This demystifying also concerns calling things in an objective manner, no more ugly, bad, nice etc.
- Here is when the concept of a [Code Smell helps](https://sourcemaking.com/refactoring).
- Now instead of reading the code base and trying to find them, lets use automatic tools to do it, for example [Metric Fu](https://github.com/metricfu/metric_fu/).

```
▶ be --verbose reek app/lib/tigo/service_manager.rb
app/lib/tigo/service_manager.rb -- 5 warnings:
  [13, 15]:Mars::ServiceManager#self.from_code calls response[:msisdn] twice (DuplicateMethodCall)
  [2]:Mars::ServiceManager#self.from_code has approx 7 statements (TooManyStatements)
  [18]:Mars::ServiceManager#self.from_msisdn has 5 parameters (LongParameterList)
  [32, 51]:Mars::ServiceManager#self.get_class calls fail(MoonExceptions::InvalidServiceError) twice (DuplicateMethodCall)
  [31]:Mars::ServiceManager#self.get_class has approx 14 statements (TooManyStatements)%
```

- [File example](https://www.evernote.com/l/AD4LzMUZKTJIQ4LzO0sPDbuWCY5SAN_L8SU).
- Now you can look up what they are and the web or the [reek documentation](https://github.com/troessner/reek/blob/master/docs/Code-Smells.md)
and understand suggestions on how to fix them.
- This is a passive approach that you can follow every day by following the [Boy Scout Rule](https://learning.oreilly.com/library/view/97-things-every/9780596809515/ch08.html)
- I don't do this as often as I would mostly because it is not as exciting.

## Understand the code base when doing small/silly/menial changes
- This is my favorite way of exploring since its driven by an external request.
- It implies a few steps:
  1. Understand the problem.
  2. Evaluate my understanding of how to implement the fix.
  3. Getting to know the code, I gave a talk on this topic some time ago [Approaching Legacy Systems](http://chischaschos.github.io/post/fog-city-ruby-legacy-apps/).
  4. Ask yourself would it be easier if this code where implemented in a different way?
      - Evaluate if it is possible to improve the areas that I'm gonna touch.
      - Evaluate an approach on how to improve it.
      - Implement improvement.
      - No back to the original problem.
  4. Implement a fix.

Let's see an example:

https://docs.google.com/presentation/d/1vjc__oqb34fET4083xa9uxh99gOvTJinRrDqpiyz4GY/edit#slide=id.p


## I'm bored or I don't have time
- Boredom, it happens to me all the time, some say it is good such [Sandi Mann the author of The Upside of Downtime: Why Boredom Is Good](https://time.com/5480002/benefits-of-boredom/).
- Some like [Cal Newport in his book Deep Work](https://medium.com/@nina.semczuk/5-practices-from-deep-work-by-cal-newport-thatll-change-your-life-303847ec5f3c)
say it is because we live in an time when everything is fighting for our attention.
- For me I doubt myself all of the time, maybe it is the [Impostor Syndrome](https://en.wikipedia.org/wiki/Impostor_syndrome),
maybe the company culture, maybe the isolation, I don't know.
- In order to challenge myself, I take notes and track my times using different tools such as:
  - http://wakatime.com
  - https://toggl.com
- I discovered a few things:
  - I have enough time to code more if I wanted and could.
  - Half or more of my time is used to figure out what to do.

## End of talk
- There is that, every one can figure out what is right for you, for me the right thing is coding.
- I love coding, I love puzzles, I love figuring out things.
- If have to look back at my life, these are the skills that have opened most of the doors:
  - Curiosity, understand problems, simplify
  - For me reflected as coding.
- Thank you for listening and letting me share this with you ![](https://media.giphy.com/media/3oEjHSeZsHu8ttM19u/giphy.gif)
