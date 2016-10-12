ugata's "for dummies" Docker Rails Foundation App

Docker ft. Rails 5, PostgreSQL, Redis, Sidekiq, Unicorn

## Getting Started

### Pre-requisites
1. Download Docker (https://www.docker.com)
2. Make sure docker is running (will vary depending on the platform - Linux vs macOS vs Windows)

### Create docker volumes
In the docker-compose.yml file, we're referencing volumes that do not exist. We can create them by running:

```
$ docker volume create --name app-postgres
$ docker volume create --name app-redis
```

When data is saved in PostgreSQL or Redis, it is saved to these volumes on your work station. This way, you won't lose your data when you restart the service because Docker containers are stateless.

### Initialize the DB
OSX/Windows users will want to remove `--­­user "$(id -­u):$(id -­g)"`

```
$ docker­-compose run --­­user "$(id ­-u):$(id -­g)" app rake db:reset
$ docker­-compose run --­­user "$(id ­-u):$(id -­g)" app rake db:migrate
```

### Get in there
```
$ docker-compose up
```

## Background Reading

### What is Docker?

A (virtualization) platform that packages an app or service with its dependencies - code, runtime, libraries, basically everything that you would need to install it on a server.

### What's the difference between Docker & VMs?

Not much, they both do isolation but there are specific tradeoffs:

VMs:
- Require a guest operating system for each isolated application
- Longer startup time to boot VM
- VMs can potentially be GBs in size

Docker:
- Shares host's kernel
- Isolation is done using linux kernel libraries (cgroups)
- Lightweight, milliseconds for bootime, low storage overhead

### Why should I use it?

1. Encapsulates application, such that moving it between environments is simple. Build once, run anywhere mantra.

2. No more pain setting up someone's dev environment, just plug and play a docker image.

3. Docker makes microservices look easy.

4. Scale up fast - images start up very fast, deployments are more predictable and resilient.

### What is Docker Compose?

Docker Compose allows you to run 1 or more Docker containers easily. You can define everything in YAML and commit this file so that other developers can simply run docker-compose up and have everything running quickly.

Reference: https://semaphoreci.com/community/tutorials/dockerizing-a-ruby-on-rails-application
