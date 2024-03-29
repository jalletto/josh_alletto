---
title: "Published Articles"
draft: false
---

## [Let's Learn About Systemd Units](https://earthly.dev/blog/systemd/)

Lastly, since I know you were wondering what happens if you run sudo kill -9 1, I can tell you that on an Ubuntu EC2 machine, it does not trigger kernel panic or cause an explosive fire at Amazon, which is a bummer.

## [Postgres Row-Level Security in Python and Django](https://pganalyze.com/blog/postgres-row-level-security-django-python)

What's nice about RLS is that if a user tries to select or alter a row they don't have access to, their query will return 0 rows, rather than throwing a permissions error. This way, a user can use `select * from table_name`, and they will only receive the rows they have access to with no knowledge of rows they don't.

## [Nix Turns 20. What the Hell Is It?](https://earthly.dev/blog/what-is-nix/)

I spoke with a wide range of Nix users, from consultants, to hobbyists, to devs using Nix in production, and even a Nix contributor. From these discussions I learned that, though Nix can refer to many different technologies, there’s a central idea that’s at the core of everything.

## [Building Pong in Your Terminal](https://earthly.dev/blog/pongo/)

I barely know Golang and I don’t know anything about game development or design. I built this to learn!

## [Using Homebrew on M1 Mac](https://earthly.dev/blog/homebrew-on-m1/)

Homebrew made some changes to where it installs packages if you are running it on a new M1 Mac, and these changes may throw you for a loop if you’re moving over to an M1 from Intel. In this article I’ll talk about what changed and why it changed. I’ll also walk you through getting all your Homebrew packages from your Intel Mac reinstalled on your M1, and share a couple of issues I came across after migrating that will hopefully help you with any gotchas you encounter in the future.

## [PostgreSQL Partitioning in Django](https://pganalyze.com/blog/postgresql-partitioning-django)

Postgres 10 introduced partitioning to improve performance for very large database tables. You will typically start to see the performance benefits with tables of 1 million or more records, but the technical complexity usually doesn’t pay off unless you’re dealing with hundreds of gigabytes of data.

## [Make a Multiplayer Game with Kaboom.js and Heroic Labs](https://blog.replit.com/make-a-multiplayer-game-with-kaboom-and-heroic-labs)

With Kaboom, a JavaScript game-programming library that helps you quickly make fun games, and Nakama, an open source distributed server created by Heroic Labs, you can easily create a multiplayer game that runs on Replit.

## [GitHub Actions and Codecov](https://about.codecov.io/blog/python-code-coverage-using-github-actions-and-codecov/)

In this tutorial, we’ll use a Python package called Coverage to generate a code coverage report locally. Then we’ll utilize the power of Codecov along with GitHub Actions to integrate our coverage report into our pull requests. The code repository is available here or you can follow along to replicate it yourself.

## [OOMKilled: troubleshooting Kubernetes memory requests and limits](https://www.airplane.dev/blog/oomkilled-troubleshooting-kubernetes-memory-requests-and-limits)

In this article, we’ll take a closer look at the OOMKilled error in K8s, why this error occurs, how to troubleshoot it when it happens, and what steps you can take to help prevent it.

## [Creating Custom Postgres Data Types in Rails](https://pganalyze.com/blog/custom-postgres-data-types-ruby-rails)

Postgres ships with the most widely used common data types, like integers and text, built in, but it's also flexible enough to allow you to define your own data types if your project demands it.

## [How to Write Effective Technical Articles](https://draft.dev/learn/posts/technical-tutorials)

Just because you know a topic well doesn’t mean you’ll crank out a great tutorial on the subject. Here are a few things to keep in mind to help you write informative, engaging, and useful technical tutorials.

## [Custom Postgres Data Types In Django](https://pganalyze.com/blog/custom-postgres-data-types-django-python)

No matter how you decide to define your datatype, Django has the functionality to allow you to map custom column data to model attributes. You can achieve this by extending the Django field class. In this walkthrough, we'll see how to create custom types in Postgres and then use them in Django to ensure consistent data types across your application. We will do this by walking you through an example project.
