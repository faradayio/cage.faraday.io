---
layout: default
title: Basics
order: b
body_id: basics
---
{::options parse_block_html="true" /}

<section class="intro">
## **Basics** — The Cage way
</section>

<section>
## Standardized application structure

Much like the "Rails way" defined a convention for structuring a standalone web application, Cage offers a uniform format for describing a broad class of multi-service Docker applications.

```
myproject/          
├── config
│   └── project.yml 
└── pods
    ├── common.env  
    ├── frontend.yml
    └── overrides   
        └── test
            └── common.env
```

* The `config/` dir contains a `project.yml` for specifying your application-level Cage config.

* The `pods/` dir is the heart of the Cage repo, containing a `.yml` file for each pod of services in your application.

* Each `pods/podname.yml` file lists and describes the one or more services included in that pod. Each service description includes a Docker image name, repo location, ports, and a service-level "API" for testing and shell access.

* The `pods/common.env` file specifies environment variables that are made available to all services in any pod. Each pod can also have its own `pods/podname.yml` file for pod-specific environment variables.

* Finally, the `overrides/` dir contains a subdir for each target that requires special overrides, either to the pod definition or environment variables.
</section>

<section>
## Key terms

Cage draws its terminology from the container ecosystem, including Docker and Kubernetes.

* A **service** is an individual component of your application, generally represented in version control as a single repository. Each service must include a `Dockerfile` that allows it to be loaded into a container.

* A **container** follows the standard Docker definition, wrapping an individual service for use in the application.

* A **source** is where a service's code is found—this could be a pre-built container image or a local source tree.

* A **target** is also known as an "environment," although we avoid that term to reduce confusion with *environment variables*, which Cage actively manages. Common targets include `development` (your laptop), `test` (automated builds on CI services), and `production`.

* A **pod** is a tightly-linked group of containers that are always deployed together. Kubernetes offers a [more complete definition](http://kubernetes.io/docs/user-guide/pods/). If you're using Amazon's ECS, a pod corresponds to an ECS "task" or "service". If you're using Docker Swarm, a pod corresponds to a single docker-compose.xml file full of services that you always launch as a single unit.

</section>

Next: [Setup](/setup)