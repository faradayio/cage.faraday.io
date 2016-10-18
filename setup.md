---
layout: default
title: Setup
order: c
body_id: setup
---
{::options parse_block_html="true" /}

<section class="intro">
## **Setup** — How to use Cage with your application
</section>

<section>
## Evaluate your application

The first step is to make sure your application is appropriate for Cage. Does it meet the following criteria?

* Multiple Dockerized services, each with their own repo on GitHub
* ...

</section>

<section>
## Installing Cage

If you've never used Cage before, you'll need to install it on your development machine.

1. First, you need to [install Docker](https://www.docker.com/products/docker) and make sure that you
have [at least version 1.8.1](https://docs.docker.com/compose/install/) of Docker Compose.

2. Download the [latest `cage` binary for your platform](https://github.com/faradayio/cage/releases/latest).

3. Install it:

``` shell
$ unzip cage-*.zip
$ sudo cp cage /usr/local/bin/
$ rm cage-*.zip cage
```
</section>

<section>
## Create a Cage repo

With Cage, you define the *structure* of your application in code. We recommend you store it in its own repo in version control—this lets you track changes and collaborate with your colleagues.

The `cage new` command lets you "stub out" a skeleton Cage application:

``` shell
$ cd ~/code
$ cage new myproject
$ cd myproject/
$ git init
$ tree
.
├── config
│   └── project.yml
└── pods
    ├── common.env
    ├── db.metadata.yml
    ├── db.yml
    ├── frontend.yml
    ├── migrate.metadata.yml
    ├── migrate.yml
    └── overrides
        ├── development
        │   └── common.env
        ├── production
        │   └── common.env
        └── test
            └── common.env
```

The `cage new` command starts us off with a few pods that serve as examples.

* `frontend` is a simple front-end web pod that includes a single service—which happens to use Rails but could use any web framework (e.g. Express). `frontend.yml` describes that pod and the Rails-based `web` service in it. Comments in the file explain its structure.

* `db` is a straightforward database pod that includes a single Postgres service. `db.yml` describes the pod and its Postgres service. The associated `db.metadata.yml` indicates that this pod should only be started in the development target.

* `migrate` is a "task pod" — its `migrate.yml` file actually references the same image as `frontend`, but with a `command` key that executes a task defined in that container. The associated `migrate.metadata.yml` declares this "task" purpose.

</section>

<section>
## Explore the sample application

After using the `cage new` command as described above, we can explore the sample application that Cage generates in your new Cage repo.

``` shell
cd ~/code/myproject
```

First, let's pull any associated Docker images listed in your Cage config:

``` shell
$ cage pull
```

Let's bring up our multi-service application!

``` shell
$ cage up
$ open http://localhost:3000
```

What if we want to make changes to one of the application's services, while using pre-built Docker images for everything else? We use the `cage mount` command to "check out" the service's source locally.

``` shell
$ cage mount web
$ $EDITOR src/web
```

After we've made some changes, we'll want to run tests:

``` shell
$ cage test web
```

We may have to run some one-time tasks as well:

``` shell
$ cage exec web rake db:create
```

Some one-time tasks are important enough to have a task pod defined, such as `migrate` in the example application:

``` shell
$ cage run migrate
```

And there are always times when you just need to shell into the service:

``` shell
$ cage shell web
```

You can always check on the status of each of your application's services like so:

``` shell
$ cage repo list
rails_hello               https://github.com/faradayio/rails_hello.git
  Cloned at src/rails_hello
```
</section>

<section>
## Describe your application

* At the very least, you'll need a single `pods/master.yml` that lists and describes each of the services in your application. But we recommend thinking carefully about the relationships between your services and separating them into logical pods, each with its own `pods/podname.yml` file.

* You'll also likely want a `pods/common.env` that defines environment variables for your application.

</section>

Next: [Setup](/setup)