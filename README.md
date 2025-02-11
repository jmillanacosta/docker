[![docker stars](https://img.shields.io/docker/stars/bigcatum/bridgedb.svg?style=flat-square)](https://hub.docker.com/r/bigcatum/bridgedb)
[![docker pulls](https://img.shields.io/docker/pulls/bigcatum/bridgedb.svg?style=flat-square)](https://hub.docker.com/r/bigcatum/bridgedb)
[![Docker build](https://github.com/bridgedb/docker/actions/workflows/buildandpush.yml/badge.svg)](https://github.com/bridgedb/docker/actions/workflows/buildandpush.yml)

# Welcome to the BridgeDb Docker repository!

BridgeDb is a framework for finding and mapping equivalent database identifiers. For local use, we work on a docker image. This docker image is located on DockerHub: https://hub.docker.com/r/bigcatum/bridgedb/

For Windows users, it is required to download Docker (https://www.docker.com/get-docker) and Docker toolbox (https://docs.docker.com/toolbox/overview/). Afterwards, start the docker quickstart terminal and let is set up a VirtualBox environment.

## Project status

<img src="https://jenkins.bigcat.unimaas.nl/view/GUI%20tests/job/Docker%20test/badge/icon">

## Pulling the image from DockerHub

To pull the image from dockerhub, enter the following line:

    docker pull bigcatum/bridgedb

At the moment, this docker image is being developed and refined for OpenRiskNet, for which the latest version is indicated in the Docker image tag. To pull this docker image and test it, use the following command:

    docker pull bigcatum/bridgedb:[tag]

This step might take some time, depending on the amount of mapping datasets (.bridge-files) are being downloaded during this step. Currently, the main docker image consists of identifier mapping (.bridge) files for human, mouse and rat.

To confirm that this step has worked and the docker image is pulled correctly, enter the following:

    docker images

This will give an overview of all images that are downloaded and ready for use.

## Running the docker image in a container

For windows users, it is necessary to know the IP adress of the VirtualBox environment, which you can find by using the following command:

    docker-machine ip

To run the docker image that you just downloaded, the following line should be entered:

    docker run -p 8080:8080 -p 8183:8183 bigcatum/bridgedb:[tag]

## Opening the docker image in a browser

To enter the docker image in a browser, Windows users should enter the IP-adress of the VirtualBox, vollowed by ':8080/swagger/'. On Linux, the docker image can be entered by writing 'http://localhost:8080/swagger/' in the browser.

## Stopping the running container

To stop a container with the running image, one needs to know the container ID, which can be found with the following command:

    docker ps -a
    
This command shows all containers on your VirtualBox Environment, including their IDs. To stop a container, enter:

    docker stop [container ID]

## Customizing the Docker container

If you wish to change the BridgeDb mapping databases the Docker image uses, you can change files with:

```shell
docker run bigcatum/bridgedb:[tag]
docker exec -i -t [container ID] bash
```

Files of interest are then the `/setup.sh`, `/var/www/html/swagger/swagger.json` and
`/opt/bridgedb/bridgedb-2.3.1/gdb.config` where you may wish to comment out the metabolite
section of the Swagger configuration if you do not host metabolite identifiers,
or download additional or newer mapping file.

Once you are happy with the customized docker, you can stop the container and build a Docker image from the container with:

```shell
docker stop [container ID]
docker commit [container ID] bigcatum/bridgedb:tag
```

This new tagged image can then be pushed to DockerHub (assuming you are logged in) with:

```shell
docker push bigcatum/bridgedb:[tag]
```

## Creating a new Docker image

When creating a new BridgeDb Docker image, update the `/setup.sh` for BridgeDb version and mapping files, `Dockerfile` for BridgeDb version, and `/opt/bridgedb/bridgedb-2.3.1/gdb.config` for BridgeDb mapping files.
