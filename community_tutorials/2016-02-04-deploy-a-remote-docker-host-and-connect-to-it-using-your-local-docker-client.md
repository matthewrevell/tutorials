---
title: "Deploy a remote Docker Host and connect to it using your local Docker Client"
slug: "deploy-a-remote-docker-host-and-connect-to-it-using-your-local-docker-client"
meta-desc: "With docker-machine you can quickly start a Cloud Instance with a pre-loaded Docker host and control it from your local Docker client."
date: 2016-02-04 12:28 +01:00
tags: "docker"
---

You can use Docker Machine to easily deploy an Instance with a running Docker Host on Exoscale. Instead of using Vagrant or the Docker toolbox to install the host locally, you use Docker Machine to spawn a new instance, install Docker on it, configure the local Docker client to talk to the Docker host and provide a number of commands for managing it.

## Spawn an Instance with a running Docker Host

Start by retrieveing the necessary API keys in [your account screen](https://portal.exoscale.ch/account/profile/api) and set the corresponding `EXOSCALE_API_KEY` and `EXOSCALE_API_SECRET` environment variables:

    export EXOSCALE_API_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    export EXOSCALE_API_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

You need to download the docker-machine binary corresponfing to your local computer architecture. For example:

    $ curl -L https://github.com/docker/machine/releases/download/v0.6.0/docker-machine-`uname -s`-`uname -m` > \
         docker-machine
    $ chmod +x docker-machine
    $ ./docker-machine --version
      docker-machine version 0.6.0, build e27fb87

If everything looks good you can now deploy your Docker host. Machine will automatically set up a new SSH key for you and deploy a new Instance.
You can specify the desired instance size via command line options or environment variables. You'll find a detail reference on the [official documenation of the Exoscale provider for docker-machine](https://docs.docker.com/machine/drivers/exoscale/).

    $ ./docker-machine create -d exoscale foobar
    Running pre-create checks...
    Creating machine...
    [...]
    Docker is up and running!
    To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: ./docker-machine env foobar

You can verify that everything worked out smoothly having a peek in the  console: you should find your new Instance and a new SSH key.

To configure your local Docker client to use this remote Docker host, simply execute the command that machine suggested you in the last output.

    $ ./docker-machine env foobar
      export DOCKER_TLS_VERIFY="1"
      export DOCKER_HOST="tcp://185.19.28.223:2376"
      export DOCKER_CERT_PATH="/Users/sebastiengoasguen/.docker/ machine/machines/foobar"
      export DOCKER_MACHINE_NAME="foobar"
      # Run this command to configure your shell:
      # eval "$(docker-machine env foobar)"

    $ eval "$(./docker-machine env foobar)"
    $ docker ps

    CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES

Congratulations your host is now up and running and you can connect and operate on it from your local Docker cli!

## Manage the Instance and connect to your Docker Host

Docker Machine offers you also basic management capabilities: start, stop, rm, and so forth. Run docker-machine with no arguments to get a full man page with all possible commands:

    $ ./docker-machine

For instance, you can list the machine you created previously, obtain its IP address, and connect to it via SSH:

    $ ./docker-machine ls
    NAME  ACTIVE  DRIVER    STATE   URL                      SWARM   DOCKER    ERRORS
    foobar  *   exoscale  Running   tcp://185.19.28.223:2376         v1.10.2

    $ ./docker-machine ip foobar
    185.19.28.223

    $ ./docker-machine ssh foobar
    Welcome to Ubuntu 15.10 (GNU/Linux 4.2.0-16-generic x86_64)
    ...

    ubuntu@foobar:~$


## Where to go from there
The official documentation gives you all the details and the options of this workflow:
[Docker Machine documentation](https://docs.docker.com/machine/)
[Exoscale driver documentation](https://docs.docker.com/machine/drivers/exoscale/)

You can also have a look at our video tutorial on how to deploy a [Docker Swarm with Docker Machine](https://www.exoscale.ch/syslog/2015/06/23/deploy-docker-swarm/) which extends those concepts further.
