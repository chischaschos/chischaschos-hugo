+++
date = "2016-08-09T07:53:33-07:00"
title = "Virtualize Rails dev environment"
draft = true

+++

## What I’m looking for my development environment
- Fast setup
- Transparent development flow
- Light and fast execution, as close as running it in my own host

## What options I am considering
- Docker
- Vagrant

## What is a win?
- I can easily start style gallery with all its required services
- I can run tests in the docker
- I can run rakes in the docker

## Extra points
- Easily mimic another environment (stage, production)

## Vagrant cons
- When iterating its build it gets really slow compared to a docker that caches steps
