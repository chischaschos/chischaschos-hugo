+++
date       = "2016-02-07T20:49:58-06:00"
title      = "RSpec and the Four Phase Tests"
tags       = ["rspec", "testing", "best practices"]
categories = ["pending-research"]
+++

## Motivation
I want to know what are the best practices to write RSpec tests when striving for a Four Phase Test.

## What the internet says about it?
- https://thoughtbot.com/upcase/videos/four-phases-of-testing.
- https://robots.thoughtbot.com/how-we-test-rails-applications
- http://code.tutsplus.com/articles/rspec-testing-for-beginners-part-1--cms-26716
- http://www.justinweiss.com/articles/writing-better-tests-with-the-three-phase-pattern/

## Notes
- `let` may be lazy and convenient.
- If another developer comes and looks at the `it`, he will see a mystery guest.
- You create a DSL within RSpec, DSL puzzle.
- Each piece of indirection improves high level view, but makes it more complex when you have to change it.
- Tests start by setting up the test and then abstract enough so you remove the unimportant details into a factory.
- Start with methods, then classes if shared among tests, then the big guns like `let` and finally shared examples.
- You don't want to rebuild what a `let` does, like memoize.
- If you see duplication in your tests it may mean a DRY design issue.
- `subject` is a less descriptive version of `let`, a `let` without a name.
- `subject` is good for [shoulda](https://github.com/thoughtbot/shoulda) matchers, not descriptive, good for one liners.

## Personal experience
When I read a test I want to understand its structure and dependencies, but
the most important thing I want is to to understand what is the behavior
that a test exercises.

The usual path to understand tests is to:

  - Run specs formatted as documentation
  - For a specific test:
    - Identify how to trigger/exercise the test
    - Understand the expectations of the test
    - Ponder if the description and context semantically matches what we
    are exercising in the test
  - Additionally for a specific test:
    - Understand what is the minimum required data to make it pass
    - Identify what are the dependencies and preconditions

RSpec can be abused as any other tools, I don't see how declaring nested lets or multiple lets that expand across 40 lines would help me understand
the aforementioned ideas. At the same time if I see multiple lets it
suggests me that all of them are important to understand the expectations
and purpose of a test, and if this is true, then the test may be too
complex and have many dependencies.

That said, is there another way?

There is, we could use cucumber to express the intentions of the code in
plain english, but then we are talking about introducting a new tool, and
we don't need it, we already have ruby, why don't we simply do ruby?

This means that we can still use rspec and use verbose function names
and high level abstraction concepts in single specifications.

An specification only introduces the high level dependencies that are
needed to execute our test and it also introduces high level expectations
definitions. Every other detail is gonna live inside ruby functions that
will play the role of a domain data factory library.
