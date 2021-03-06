---
id: version-2.0.0-installation-reaction-platform
title: Install with Reaction Platform
sidebar_label: Install with Reaction Platform
original_id: installation-reaction-platform
---


The Reaction Platform is the easiest way to run the entire suite of Reaction services at once, as of Reaction version 2.0. The Platform installs and runs the entire suite of Reaction services in these directories:

| Directory                                                                                  | Services                                   |
| ------------------------------------------------------------------------------------------ | ------------------------------------------ |
| [`reaction`](https://github.com/reactioncommerce/reaction)                                 | GraphQL API, Meteor API, Classic UI, Mongo |
| [`reaction-hydra`](https://github.com/reactioncommerce/reaction-hydra)                     | Authentication server, PostgreSQL          |
| [`reaction-next-starterkit`](https://github.com/reactioncommerce/reaction-next-starterkit) | Next.js storefront application             |

## Installation

1. Requirements:

- Install Docker and Docker Compose ([Mac](https://docs.docker.com/docker-for-mac/install/) | [Windows](https://docs.docker.com/docker-for-windows/install/) | [Linux](https://docs.docker.com/compose/install/#install-compose))
- Install [Node.js](https://nodejs.org/en/)
- A GitHub account with a <a href="https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/">configured SSH key</a>

> **Windows**: Reaction Platform has not been fully tested on Windows at this time.

> **Linux**: Docker Compose is included when installing Docker on Mac and Windows, but will need to be installed separately on Linux.

2. Before you get started, make sure you are not running any applications on the default ports: `3000`, `4000`, `4444`.

3. Clone [**Reaction Platform**](https://github.com/reactioncommerce/reaction-platform)

```sh
git clone git@github.com:reactioncommerce/reaction-platform.git
```

4. Now you're ready to install. Run this command to bootstrap and start all of the services:

```sh
cd reaction-platform
make
```

This process may take some time. The Platform is checking that dependencies are present, cloning the sub-projects from GitHub, downloading and building Docker images, and starting services.

By the end of the initial `make` process, you should see these log messages:

```sh
Successfully tagged reaction-next-starterkit_web:latest
Running post-build hook script for reaction-next-starterkit.
reaction-next-starterkit post-build script invoked.
Recreating reaction-next-starterkit_web_1 ... done
Running post-project-start hook script for reaction-next-starterkit.
reaction-next-starterkit post-project-start script invoked.
Running post-system-start hook script for reaction-hydra.
reaction-hydra post-system-start script invoked.

No post-system-start hook script for reaction. Skipping.
Running post-system-start hook script for reaction-next-starterkit.
reaction-next-starterkit post-system-start script invoked.
```

You can confirm that all of these containers have installed and are starting up by running:

```sh
docker ps
```
You should see a log of all running containers with names, ports and statuses:

```sh
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                                                      NAMES
7616b75a7277        reaction-next-starterkit_web      "sh -c 'yarn install…"   15 minutes ago      Up 15 minutes       0.0.0.0:4000->4000/tcp                                     reaction-next-starterkit_web_1
56053f0275fb        reaction_reaction                 "bash -c 'npm instal…"   18 minutes ago      Up 18 minutes       0.0.0.0:3000->3000/tcp                                     reaction_reaction_1
31f4180746a4        mongo:3.6.3                       "docker-entrypoint.s…"   2 weeks ago         Up 18 minutes       0.0.0.0:27017->27017/tcp                                   reaction_mongo_1
1f420dd7c664        oryd/hydra:v1.0.0-beta.9-alpine   "hydra serve all --d…"   2 weeks ago         Up 19 minutes       0.0.0.0:4444-4445->4444-4445/tcp, 0.0.0.0:5555->5555/tcp   reaction-hydra_hydra_1
fc43e0a4d55b        postgres:10.3                     "docker-entrypoint.s…"   2 weeks ago         Up 19 minutes       0.0.0.0:32769->5432/tcp                                    reaction-hydra_postgres_1
```

5. As the various repositories are downloading and services are starting, check the logs by running `docker-compose logs -f` in the specific repository. For example, to display the `reaction` logs, run:

```sh
cd reaction
docker-compose logs -f
```

To display the `reaction-next-starterkit` logs, run:

```sh
cd reaction-next-starterkit
docker-compose logs -f
```

6. Once all of the services are running, the following will be accessible at these URLs:

| Directory: Service                                                                         | URL                                                           |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |
| [`reaction`](https://github.com/reactioncommerce/reaction): GraphQL API                    | [localhost:3000/graphql-alpha](localhost:3000/graphql-alpha) |
| [`reaction`](https://github.com/reactioncommerce/reaction): GraphQL API playground         | [localhost:3000/graphiql](localhost:3000/graphiql)            |
| [`reaction`](https://github.com/reactioncommerce/reaction): Classic UI                     | [localhost:3000](localhost:3000)                              |
| [`reaction`](https://github.com/reactioncommerce/reaction): MongoDB                        | [localhost:27017](localhost:27017)                            |
| [`reaction-hydra`](https://github.com/reactioncommerce/reaction-hydra): oryd/hydra         | [localhost:4444](localhost:4444)                              |
| [`reaction-next-starterkit`](https://github.com/reactioncommerce/reaction-next-starterkit) | [localhost:4000](localhost:4000)                              |

7. Congrats 🎉  Now you're running the entire suite of Reaction Platform services and ready to start developing.

## Developing with Reaction Platform

Once you've bootstrapped the entire Reaction development in Docker, use `make start` to start all containers.

### Reaction Platform commands

Run these commands from the `reaction-platform` directory:

| Command                    | Description                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------- |
| `make`                     | Boostraps the entire Reaction development environment in Docker.                      |
| `make stop`                | Stops all containers.                                                                 |
| `make start`               | Starts all containers.                                                                |
| `make rm`                  | Removes all containers. Volumes are not removed.                                      |
| `make clean`               | Removes all containers, networks, and volumes. Any volume data will be lost.          |
| `make init-<project-name>` | Example: `make init-reaction-next-starterkit`. Does clone/setup for a single project. |

Learn more about [Reaction Platform](https://github.com/reactioncommerce/reaction-platform).

### Docker commands

Learn more about [developing in Docker](./installation-docker-development#run-the-apps) and [troubleshooting Docker development](./troubleshooting-development#docker-issues).

### Git

Each sub-project has its own respective Git repository. If you want to run a different branch of `reaction` against `reaction-next-starterkit`, for example, change directories into `reaction` and `git checkout` a different branch there.
