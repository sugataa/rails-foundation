# Sugata's "for dummies" Docker + Rails Foundation App

So, you know how to create regular ol' Rails app...but you want to make that same Rails app slick and **dockerized**. Well then, this is the **perfect foundation** for you.

All you need is to do is:
- clone this repo
- copy over the contents to your rails folder
- finish the remaining steps

You are getting the following tech:

| Tech              | Are           | Version  |
| ----------------- |:-------------:| --------:|
| Container         | **Docker**    |  1.12.1  |
| Backend           | **Rails 5**   |  5.0.0.1 |
| Database          | **PostgreSQL**|  9.6.0   |
| Key-Value Store   | **Redis**     |  3.2.0   |
| Background Worker | **Sidekiq**   |  4.2.2   |
| HTPP Server       | **Unicorn**   |  5.1.0   |

## Getting Started

### Pre-requisites
1. Download Docker (https://www.docker.com)
2. Make sure docker is running (will vary depending on the platform - Linux vs macOS vs Windows)
3. Clone this repo and copy over the contents to your application repo

### Copy over example files
```shell
$ cp .rails-foundation.env.example .<REPLACE WITH YOUR REPO NAME>.env
$ cp docker-compose.yml.example docker-compose.yml
```

### Create docker volumes
In the docker-compose.yml file, we're referencing volumes that do not exist. We can create them by running:

```shell
$ docker volume create --name <REPLACE WITH REPO NAME-postgres i.e. rails-foundation-postgres>
$ docker volume create --name <REPLACE WITH REPO NAME-redis i.e. rails-foundation-redis>
```

When data is saved in PostgreSQL or Redis, it is saved to these volumes on your work station. This way, you won't lose your data when you restart the service because Docker containers are stateless.

### Initialize the DB
OSX/Windows users will want to remove `--­­user "$(id -­u):$(id -­g)"`

```shell
$ docker­-compose run --­­user "$(id ­-u):$(id -­g)" <REPLACE WITH REPO NAME i.e. rails-foundation> rake db:reset
$ docker­-compose run --­­user "$(id ­-u):$(id -­g)" <REPLACE WITH REPO NAME i.e. rails-foundation> rake db:migrate
```

### Get in there
```
$ docker-compose up
```

Navigate to `0.0.0.0:8000` on your favourite browser, and you should be greeted by:

![Started Rails](https://raw.githubusercontent.com/sugataa/rails-foundation/master/public/complete.png)

If everything is working as expected, you should also see the following:

```shell
$ docker ps

CONTAINER ID        IMAGE                              COMMAND                  CREATED             STATUS              PORTS                    NAMES
64be678f7874        railsfoundation_sidekiq            "bundle exec sidekiq "   11 hours ago        Up 4 minutes                                 railsfoundation_sidekiq_1
dee6bfe41f0c        railsfoundation_rails-foundation   "/bin/sh -c 'bundle e"   11 hours ago        Up 4 minutes        0.0.0.0:8000->8000/tcp   railsfoundation_rails-foundation_1
0575b80cebc1        redis:3.0.5                        "/entrypoint.sh redis"   11 hours ago        Up 4 minutes        0.0.0.0:6379->6379/tcp   railsfoundation_redis_1
0e967e872a90        postgres:9.6.0                     "/docker-entrypoint.s"   11 hours ago        Up 4 minutes        0.0.0.0:5432->5432/tcp   railsfoundation_postgres_1
```

## Common Commands

See running processes:

```
$ docker ps
```

Connect to the postgres database:

```
$ psql -h localhost -p 5432
```

Log into the rails server:

```
$ docker-compose run rails-foundation /bin/bash
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

### Reference:

https://semaphoreci.com/community/tutorials/dockerizing-a-ruby-on-rails-application
