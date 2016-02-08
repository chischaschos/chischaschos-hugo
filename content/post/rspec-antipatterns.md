+++
date = "2016-02-07T20:49:58-06:00"
draft = true
title = "rspec antipatterns"

+++

- let is lazy and convenient
- if another developer comes and looks at the it, he will see mystery guests,
- you create a DSL within RSpec, DSL puzzle
- each piece of indirection improves high level view, but makes it more complex when you have to change it
- tests start by setting up the test
  - abstract enough so you remove the unimportant details into a factory
- start with methods, then classes if shared among tests, then the big guns like let, shared examples
- you don;t want to rebuild what a let does, like memoize
  - if you see duplication in your tests it may mean a DRY design issue
- subject a less descriptive version of let, a let without a name
  - good for shoulda matchers, not descriptive, good for one liners
-


