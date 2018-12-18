+++
date = "2018-12-05T10:00:00-07:00"
title = "TIL bundler binstubs with direnv"
draft = false
tags = ["til"]
+++

## Motivation
I have this ruby project where I'm enforcing [RuboCop](https://github.com/rubocop-hq/rubocop) and the way I do this is twofold:

1. by using a pre-commit hook that runs RuboCop altogether.
2. and with [neomake](https://github.com/neomake/neomake) so I can see errors/warnings right on my editor.

Today I noticed that the reported errors/warnings in my editor and the command line are not the same. What happened here?

## Is it neomake issue?
I noticed that I had different versions of rubocop installed, because of reasons different projects have different versions of RuboCop.
But shouldn't neomake use bundler exec?, well maybe https://github.com/neomake/neomake/issues/2078, but there is a less obtrusive way.

## [direnv](https://direnv.net/) and bundler binstubs
I have already installed [direnv](https://direnv.net/), so what I did is:

1. Generate the [rubocop binstub](https://bundler.io/man/bundle-binstubs.1.html):

    ```bash
    bundler binstubs rubocop
    ```

2. Add the current path's `bin` to the `PATH` env variable via my `.envrc`:

    ```bash
    export PATH=$(pwd)/bin:$PATH`
    ```

## Conclusion
This fixed the problem. Now both my vim and command line are using the same version of RuboCop.
