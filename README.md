# docker-web-app-demo

## Overview

This repository is used to create the `senzing/web-app-demo` docker image
which demonstrates the combination of two projects:

1. [senzing-api-server](https://github.com/Senzing/senzing-api-server)
1. [entity-search-web-app](https://github.com/Senzing/entity-search-web-app)

The result is that a user can run the docker container using a local
[/opt/senzing](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/create-senzing-dir.md)
and visualize results with the "Entity Search Web App".

### Related artifacts

1. DockerHub
    1. [senzing/web-app-demo](https://hub.docker.com/r/senzing/web-app-demo)
    1. [senzing/web-app-demo-unstable](https://hub.docker.com/r/senzing/web-app-demo-unstable)

### Contents

1. [Expectations](#expectations)
    1. [Space](#space)
    1. [Time](#time)
    1. [Background knowledge](#background-knowledge)
1. [Demonstrate using Docker](#demonstrate-using-docker)
    1. [Create SENZING_DIR](#create-senzing_dir)
    1. [Configuration](#configuration)
    1. [Run docker container](#run-docker-container)
1. [Develop](#develop)
    1. [Prerequisite software](#prerequisite-software)
    1. [Clone repository](#clone-repository)
    1. [Build docker image for development](#build-docker-image-for-development)
1. [Errors](#errors)

## Expectations

### Space

This repository and demonstration require 6 GB free disk space.

### Time

Budget 20 minutes to get the demonstration up-and-running, depending on CPU and network speeds.

### Background knowledge

This repository assumes a working knowledge of:

1. [Docker](https://github.com/Senzing/knowledge-base/blob/master/WHATIS/docker.md)

## Demonstrate using Docker

### Create SENZING_DIR

1. If `/opt/senzing` directory is not on local system, visit
   [HOWTO - Create SENZING_DIR](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/create-senzing-dir.md).

### Configuration

* **SENZING_DATABASE_URL** -
  Database URI in the form: `${DATABASE_PROTOCOL}://${DATABASE_USERNAME}:${DATABASE_PASSWORD}@${DATABASE_HOST}:${DATABASE_PORT}/${DATABASE_DATABASE}`
  Default:  [internal SQLite database]
* **SENZING_DIR** -
  Path on the local system where
  [Senzing_API.tgz](https://s3.amazonaws.com/public-read-access/SenzingComDownloads/Senzing_API.tgz)
  has been extracted.
  See [Create SENZING_DIR](#create-senzing_dir).
  No default.
  Usually set to "/opt/senzing".

### Run docker container

#### Demonstrate using SQLite database

1. :pencil2: Set environment variables.  Example:

    ```console
    export SENZING_DIR=/opt/senzing
    ```

1. Run the docker container.  Example:

    ```console
    sudo docker run \
      --detach \
      --name senzing-web-app-demo \
      --publish 9002:80 \
      --restart always \
      --volume ${SENZING_DIR}:/opt/senzing \
      senzing/web-app-demo
    ```

1. "Entity Search Web App" can be viewed at [localhost:9002](http://localhost:9002), since 9002 was the published port.

1. To stop container. Example:

    ```console
    sudo docker container kill senzing-web-app-demo
    sudo docker container rm   senzing-web-app-demo
    ```

#### Demonstrate using PostgreSQL database

1. :pencil2: Set environment variables.  Example:

    ```console
    export DATABASE_PROTOCOL=postgresql
    export DATABASE_USERNAME=postgres
    export DATABASE_PASSWORD=postgres
    export DATABASE_HOST=my.postgresql.com
    export DATABASE_PORT=5432
    export DATABASE_DATABASE=G2

    export SENZING_DIR=/opt/senzing
    ```

1. Run the docker container.  Example:

    ```console
    export SENZING_DATABASE_URL="${DATABASE_PROTOCOL}://${DATABASE_USERNAME}:${DATABASE_PASSWORD}@${DATABASE_HOST}:${DATABASE_PORT}/${DATABASE_DATABASE}"

    sudo docker run \
      --detach \
      --env SENZING_DATABASE_URL="${SENZING_DATABASE_URL}" \
      --name senzing-web-app-demo \
      --publish 9002:80 \
      --restart always \
      --volume ${SENZING_DIR}:/opt/senzing \
      senzing/web-app-demo
    ```

1. "Entity Search Web App" can be viewed at [localhost:9002](http://localhost:9002), since 9002 was the published port.

1. To stop container. Example:

    ```console
    sudo docker container kill senzing-web-app-demo
    sudo docker container rm   senzing-web-app-demo
    ```

## Develop

### Prerequisite software

The following software programs need to be installed:

1. [git](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/install-git.md)
1. [make](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/install-make.md)
1. [docker](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/install-docker.md)

### Clone repository

1. Set these environment variable values:

    ```console
    export GIT_ACCOUNT=senzing
    export GIT_REPOSITORY=docker-web-app-demo
    ```

1. Follow steps in [clone-repository](https://github.com/Senzing/knowledge-base/blob/master/HOWTO/clone-repository.md) to install the Git repository.

1. After the repository has been cloned, be sure the following are set:

    ```console
    export GIT_ACCOUNT_DIR=~/${GIT_ACCOUNT}.git
    export GIT_REPOSITORY_DIR="${GIT_ACCOUNT_DIR}/${GIT_REPOSITORY}"
    ```

### Build docker image for development

1. Option #1 - Using docker command and local repository.

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo docker build --tag senzing/web-app-demo .
    ```

1. Option #2 - Using make command.

    ```console
    cd ${GIT_REPOSITORY_DIR}
    sudo make docker-build
    ```

## Errors

1. See [docs/errors.md](docs/errors.md).
