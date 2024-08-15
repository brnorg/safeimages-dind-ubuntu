# Docker-in-Docker (dind) Environment

This repository contains a Dockerfile that creates a custom Docker environment for running Docker-in-Docker (dind) setups, based on Ubuntu 20.04.

## Overview

The Docker-in-Docker (dind) setup allows you to run a Docker daemon inside a Docker container, enabling you to build, test, and run other Docker containers within your main Docker environment.

This Dockerfile creates a custom Ubuntu 20.04 image with the necessary configurations and tools to set up a dind environment. The image includes the following:

- Docker CE CLI (client) installation
- Environment variables for Docker configuration:
  - `DOCKER_TLS_VERIFY`
  - `DOCKER_CERT_PATH`
  - `DOCKER_VERSION`
  - `DOCKER_TLS_CERTDIR`
  - `DOCKER_HOST`
  - `DOCKER_BUILDX_VERSION`
  - `DOCKER_COMPOSE_VERSION`

## Usage

### docker start new oficial image container

1. create network and start docker oficial container on 27 version

  ```bash
  docker network create container-network
  docker run --privileged --name some-docker -d --network container-network --network-alias docker -e DOCKER_TLS_CERTDIR=/certs -v some-docker-certs-ca:/certs/ca -v some-docker-certs-client:/certs/client docker:27-dind
  ```

### docker client (docker-ce-cli)

1. Build the Docker image:

   ```bash
   docker build . -t dind-ubuntu
   ```

2. Run the dind container:

   ```bash
   docker run --privileged --rm -it --network container-network -e DOCKER_TLS_CERTDIR=/certs -v some-docker-certs-client:/certs/client:ro dind-ubuntu bash
   ```
   
   The `--privileged` flag is required to allow the container to access the Docker daemon.

3. Inside the dind container, you can now run Docker commands as usual:

   ```bash
   docker ps
   ```

## Benefits

- Isolated Docker environment for testing and development
- Ability to run Docker commands within the container
- Consistent and reproducible setup across different environments
- Customizable configuration options through environment variables

## Considerations

- Ensure proper security measures when using dind, as it can potentially expose your host system to risks.
- Monitor resource usage and performance, as running a Docker daemon inside a container can have an impact on system resources.
- Familiarize yourself with the best practices and potential pitfalls of Docker-in-Docker setups.

Feel free to customize the Dockerfile and the usage instructions to fit your specific needs.
