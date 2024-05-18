---
description: For local builds
---

# ⭐ Docker-compose Deployment

{% hint style="success" %}
Please see [https://github.com/UdotCASH/uchain-info//tree/master/docker-compose](https://github.com/UdotCASH/uchain-info//tree/master/docker-compose) for all required information.
{% endhint %}

### Prerequisites

* Docker v20.10+
* Docker-compose 2.x.x+
* Running Ethereum JSON RPC client

### Building Docker containers from source

```bash
cd ./docker-compose
docker-compose up --build
```

{% hint style="info" %}
**Note**: you can use `docker compose` instead of `docker-compose`, if compose v2 plugin is installed in Docker.
{% endhint %}

{% hint style="warning" %}
**Note**: if you don't need to make backend customizations, you can run `docker-compose up` in order to launch from pre-build backend Docker image. This will be much faster.
{% endhint %}

This command uses `docker-compose.yml` by-default, which builds the backend of the explorer into the Docker image and runs 9 Docker containers:

* Postgres 14.x database, which will be available at port 7432 on the host machine.
* Redis database of the latest version.
* UCHAIN.INFO backend with api at /api path.
* Nginx proxy to bind backend, frontend and microservices.
* UCHAIN.INFO explorer at http://localhost.

and 4 containers for microservices (written in Rust):

* [Stats](https://github.com/UdotCASH/uchain-info/-rs/tree/main/stats) service with a separate Postgres 14 DB.
* [Sol2UML visualizer](https://github.com/UdotCASH/uchain-info/-rs/tree/main/visualizer) service.
* [Sig-provider](https://github.com/UdotCASH/uchain-info/-rs/tree/main/sig-provider) service.

{% hint style="warning" %}
**Note for Linux users**: Linux users who run UCHAIN.INFO in docker **with a local node** need to run the node on [http://0.0.0.0/](http://0.0.0.0/) rather than [http://127.0.0.1/](http://127.0.0.1/)
{% endhint %}

### Configs for different Ethereum clients

The repo contains built-in configs for different JSON RPC clients without need to build the image.

| **JSON RPC Client**              | **Docker compose launch command**                   |
| -------------------------------- | --------------------------------------------------- |
| Erigon                           | `docker-compose -f erigon.yml up -d`                |
| Geth (suitable for Reth as well) | `docker-compose -f geth.yml up -d`                  |
| Geth Clique                      | `docker-compose -f geth-clique-consensus.yml up -d` |
| Nethermind, OpenEthereum         | `docker-compose -f nethermind up -d`                |
| Ganache                          | `docker-compose -f ganache.yml up -d`               |
| HardHat network                  | `docker-compose -f hardhat-network.yml up -d`       |

* Running only explorer without DB: `docker-compose -f external-db.yml up -d`. In this case, no db container is created. And it assumes that the DB credentials are provided through `DATABASE_URL` environment variable on the backend container.
* Running explorer with external backend: `docker-compose -f external-backend.yml up -d`
* Running explorer with external frontend: `docker-compose -f external-frontend.yml up -d`
* Running all microservices: `docker-compose -f microservices.yml up -d`

All of the configs assume the Ethereum JSON RPC is running at http://localhost:8545.

In order to stop launched containers, run `docker-compose -d -f config_file.yml down`, replacing `config_file.yml` with the file name of the config which was previously launched.

You can adjust uchaininfo environment variables:

* for backend in `./envs/common-uchaininfo.env`
* for frontend in `./envs/common-frontend.env`
* for stats service in `./envs/common-stats.env`
* for visualizer in `./envs/common-visualizer.env`

Descriptions of the ENVs are available

* for [backend](https://docs.uchain.info/for-developers/information-and-settings/env-variables)
* for [frontend](https://github.com/UdotCASH/uchain-info-frontend/blob/main/docs/ENVS.md).

### Running via Makefile

Prerequisites are the same, as for docker-compose setup.

Start all containers:

```bash
cd ./docker
make start
```

Stop all containers:

```bash
cd ./docker
make stop
```

{% hint style="warning" %}
**Note**: Makefile uses the same .env files since it is running docker-compose services inside.
{% endhint %}
