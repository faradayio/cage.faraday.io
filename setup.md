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

If you have [Rust](https://www.rust-lang.org/) installed, you can also
install it using `cargo`:

``` shell
$ cargo install cage
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
    └── targets
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

* `rake` is a "task pod" — its `rake.yml` file actually references the same image as `frontend`, but with a `entrypoint` key that makes it easy to run rake tasks in the container. The associated `rake.metadata.yml` declares this "task" purpose.

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

Let's start by bringing up our local database server and initializing our database:

``` shell
$ cage up db
$ cage run rake db:create
$ cage run rake db:migrate
```

Now let's bring up the rest of our multi-service application!

``` shell
$ cage up
```

Now we can look at our web app!

```
$ open http://localhost:3000
```

What if we want to make changes to one of the application's services, while using pre-built Docker images for everything else? We use the `cage mount` command to "check out" the service's source locally.

``` shell
$ cage source mount rails_hello
# Restart our Rails app using local source code.
$ cage up
$ $EDITOR src/rails_hello
```

After we've made some changes, we'll want to run tests:

``` shell
$ cage test web
```

Some one-time tasks are important enough to have a task pod defined, such as `rake` in the example application:

``` shell
$ cage run rake -T
```

And there are always times when you just need to shell into the service:

``` shell
$ cage shell web
```

You can always check to see what source trees are available, and which are
currently mounted into services:

``` shell
$ cage source ls
rails_hello               https://github.com/faradayio/rails_hello.git
  Cloned at src/rails_hello (mounted)
```
</section>

<section>
## Describe your application

* At the very least, you'll need a single `pods/master.yml` that lists and describes each of the services in your application. But we recommend thinking carefully about the relationships between your services and separating them into logical pods, each with its own `pods/podname.yml` file.

* You'll also likely want a `pods/common.env` that defines environment variables for your application.

</section>

Next: [On GitHub](https://github.com/faradayio/cage)
