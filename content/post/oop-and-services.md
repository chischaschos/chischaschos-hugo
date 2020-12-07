+++
date = "2018-09-08T13:46:00-07:00"
title = "Object Oriented Programming and Services"
draft = false
categories = ["pending-research"]
+++

## Motivation
- What is a service?
- Who defines it?
- What contexts can services live in?
- Services best practices around naming and design.

### Eric Evan's Domain driven design
[Service](https://en.wikipedia.org/wiki/Domain-driven_design), when an operation does not conceptually belong to any object. Following the natural contours of the problem, you can implement these operations in services. See also Service (systems architecture).

### Martin Fowler
[The Service Layer](https://martinfowler.com/eaaCatalog/serviceLayer.html), defines an application's boundary with a layer of services that establishes a set of available operations and **coordinates the application's response in each operation**.

- Pattern to organize business logic just like a Transaction Script or Domain Modal.
- Many designers  divide business logic into two kinds:
  - Domain logic, purely the problem domain
  - and Application logic, application responsibilities, workflow
- Domain logic are usually boring CRUD with behavior to manage their data
- Service adds to these domains by coordinating multiple domains and/or adding application logic
- Useful on rails apps to share logic between jobs and controllers of different types (web, api)

## SRP
- All the behavior in a class should be cohesive and working towards the same purpose, same responsibility.

## Stitchfix On Rails Service Objects

- https://multithreaded.stitchfix.com/blog/2015/06/02/anatomy-of-service-objects-in-rails/
- Design:
  - Methods should return rich result objects not booleans or primitives
  - Receive value objects
  - Dependent service objects accessible via private methods and created either constructor or lazily initialization
