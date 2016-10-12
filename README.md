# Sugata's "batteries-included" Docker + Rails App

So, you know how to create regular ol' Rails app...but you want to make that same Rails app slick and **dockerized**. Well then, this is the **perfect foundation** for you.

| Architecture      | Tech          | Version  |
|:----------------- |:-------------:| --------:|
| Container         | **Docker**    |  1.12.1  |
| Backend           | **Rails 5**   |  5.0.0.1 |
| Database          | **PostgreSQL**|  9.6.0   |
| Key-Value Store   | **Redis**     |  3.2.0   |
| Background Worker | **Sidekiq**   |  4.2.2   |
| HTPP Server       | **Unicorn**   |  5.1.0   |

## Getting Started

### Pre-requisites
- Create your Rails application folder
```shell
$ mkdir my-app
$ cd my-app
$ git init
```
- Download Docker (https://www.docker.com)
- Make sure docker is running (will vary depending on the platform - Linux vs macOS vs Windows)
- Clone this repo into the same folder where the  `my-app` folder lives and copy over the contents
```shell
$ git clone https://github.com/sugataa/rails-foundation.git
$ mv rails-foundation/* my-app/
```

### Activate example files
```shell
$ cp rails-foundation.env.example .my-app.env
$ cp docker-compose.yml.example docker-compose.yml
```

### Edit .my_app.env file
Change the name of the repo name
```shell
REPO_NAME=my-app
```
Change the name of the Database URL
```shell
DATABASE_URL=postgresql://my-app:yourpassword@postgres:5432/my-app?encoding=utf8&pool=5&timeout=5000
```

### Edit docker-compose.yml file
Change variables to match, where appropriate
```shell
<REPLACE ME WITH A VOLUME NAME i.e. rails-foundation-postgres> ====> my-app-postgres

<REPLACE ME WITH A VOLUME NAME i.e. rails-foundation-redis> ====> my-app_redis

<REPLACE ME WITH A REPO NAME i.e. rails-foundation> ====> my-app

<REPLACE ME WITH A ENV FILE NAME i.e. .rails-foundation.env> ====> .my-app.env
```

### Create docker volumes
In the docker-compose.yml file, we're referencing volumes that do not exist. We can create them by running:

```shell
$ docker volume create --name my-app-postgres
$ docker volume create --name my-app-redis
```

When data is saved in PostgreSQL or Redis, it is saved to these volumes on your work station. This way, you won't lose your data when you restart the service because Docker containers are stateless.

### Initialize the DB
OSX/Windows users will want to remove `--­­user "$(id -­u):$(id -­g)"`

```shell
$ docker­-compose run --­­user "$(id ­-u):$(id -­g)" my-app rake db:reset
$ docker­-compose run --­­user "$(id ­-u):$(id -­g)" my-app rake db:migrate
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
