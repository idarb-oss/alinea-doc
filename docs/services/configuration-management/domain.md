---
title: Domain
summary:
authors:
  - Idar Bergli
date: 2022-05-28
---

Definition:

> Domain - A sphere of knowledge, influence, or activity. The subject area to which the user applies a program is the domain of the software. Domain-Driven Design Reference, Eric Evans

The main business entities are `Client`, `Environment`, `Cluster`, `Section` and `Item`. There can be predefined `Template` created for easier setting up new `Client` in the manager.

An `Owner` can register a new `Client` and choose to use a pre-defined `Template` or go thrue an sequence of setting up needed `Environment` with its `Cluster` and `Section`.

An `Owner` can give access or ownership to other `User`s.

An `Reader` can read an client configuration settings, but see the value of `Item` that are an secret.

An `Owner` or `Editor` can define `Section` and the `Item` belonging to the sections.

An `Owner` or `Editor` can create an new `Release` for an client to change the configurations that will be used by an service/application.

## User Access

Each `Owner`, `Editor`, `Reader` is an user. To be an `User`, `User Registration` is required and confirmed.

Each `User` is assigned one or more `Roles`.

Each `Role` has a set of `Permissions`. A `Permission` defines weather `User` can invoke a particular action.

## Event Storming

Description of _behavior_ and and _Events_ that occure in the domain.

There are many ways to show behavior and events. One of them is a light technique called Event Storming which is becoming more popular.
