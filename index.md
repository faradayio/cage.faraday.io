---
layout: default
title: Introduction
order: a
body_id: home
---
{::options parse_block_html="true" /}

<section class="intro">
## **Cage** helps you develop complex, multi‑container Docker applications locally

v0.1.1 — developer preview
</section>

<section>
## What is Cage?

The Cage open-source project defines a standardized format for you to describe your application structure, then brings the whole application up on your laptop for local development. You can mix pre-built Docker images with local source trees, "checking out" individual services for code changes and testing.

</section>

<section>
## A quick tour

Define your application structure and environment variables in a new Cage repo:

``` shell
$ cage new myproject
$ tree myproject
myproject/
├── ...
└── pods
    ├── common.env
    ├── frontend.yml
    └── ...
```

Define how your application works in `frontend.yml`:

``` yaml
version: "2"
services:
  rails_hello:
    image: "faraday/rails_hello"
    build: "https://github.com/faradayio/rails_hello.git"
    ports:
    - "3000:3000"
```

Bring up the whole stack with a single command:

``` shell
$ cage up
```

Check out a single service for local development and testing:

``` shell
$ cage source mount rails_hello
# Restart the app using the cloned source code.
$ cage up
$ $EDITOR src/rails_hello
$ cage test rails_hello
# Commit and push changes.
```

Voilà!
</section>

<section>
## Why Cage?

At some point, your Docker application will begin to comprise multiple containers that work together. This is the promise of the microservice topology.

At this point, further progress can become difficult for developers. Because services depend on each other, groups of containers ("pods") must come up together for testing and local development. Each container may have different means for running tests, starting services, and opening shells.

In an environment like this, each developer is obligated to maintain expertise in the entire stack rather than focusing on her work. Cage provides a helpful alternative.
</section>

Next: [Basics](/basics)
