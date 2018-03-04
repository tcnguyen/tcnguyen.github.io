---
layout: page
title: Docker
---
- https://docs.docker.com/get-started/
- http://training.play-with-docker.com/beginner-linux/

**Basics**
```bash
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Excecute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls -all
docker container ls -a -q
```

**Run other Linux container interactively**
```bash
docker container run --interactive --tty --rm ubuntu bash
```
**Run a background MySQL container**
```bash
## -- detach will run the container in the background.
## --name will name it mydb.
## -e will use an environment variable to specify the root password
docker container run \
--detach \
--name will name it mydb.
--name mydb \
-e MYSQL_ROOT_PASSWORD=my-secret-pw \
mysql:latest
```

**Check what happening**
```bash
## logs from the MySQL Docker container.
docker container logs mydb

## processes running inside the container.
docker container top mydb
```

**Tag and push**
```bash
## Log in to the Docker public registry on your local machine.
docker login
## example docker tag friendlyhello john/get-started:part2
docker tag image username/repository:tag
##Upload your tagged image to the repository
docker push username/repository:tag
```

**Pull and run**
```bash
## If the image isn’t available locally on the machine, Docker pulls it from the repository.
docker run -p 4000:80 username/repository:tag
```

**Service and Deploy**

Services codify a container’s behavior in a Compose file, and this file can be used to scale, limit, and redeploy our app. Changes to the service can be applied in place, as it runs, using the same command that launched the service: docker stack deploy.

Some commands to explore at this stage:
```bash
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```

**Swarm**

```bash
# init swarm
docker swarm init --advertise-addr 172.17.0.1
# leave swarm
docker swarm leave --force

#create a couple of VMs using docker-machine, using the VirtualBox driver:
docker-machine create --driver virtualbox myvm1
docker-machine create --driver virtualbox myvm2
# list the machines and get their IP addresses.
docker-machine ls

# Instruct myvm1 to become a swarm manager with
#myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376
docker-machine ssh myvm1 "docker swarm init --advertise-addr 192.168.99.100"

# To add a worker to this swarm
# Always run docker swarm init and docker swarm join with port 2377 or no port
# at all and let it take default
docker swarm join --token <token> <myvm ip>:<port>

# Send command to myvm2 via docker-machine ssh to have myvm2 join your new swarm as # a worker
docker-machine ssh myvm2 "docker swarm join \
--token <token> \
<ip>:2377"

#Run docker node ls on the manager to view the nodes in this swarm:
docker-machine ssh myvm1 "docker node ls"

#Leaving a swarm
# to start over, you can run docker swarm leave from each node.
docker-machine ssh myvm2 "docker swarm leave"

# start a machine
docker-machine start <machine-name>
```

*Configure a docker-machine shell to the swarm manager*
```bash
# configures your current shell to talk to the Docker daemon on the VM
# allows you to use your local docker-compose.yml file to deploy the app “remotely”
# without having to copy it anywhere.
docker-machine env <machine>

# configure your shell to talk to myvm1.
eval $(docker-machine env myvm1)

# unset
eval $(docker-machine env -u)

# docker-machine ls to verify that myvm1 is now the active machine
docker-machine ls

```
```bash
docker-machine create --driver virtualbox myvm1 # Create a VM (Mac, Win7, Linux)
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1 # Win10
docker-machine env myvm1                # View basic information about your node
docker-machine ssh myvm1 "docker node ls"         # List the nodes in your swarm
docker-machine ssh myvm1 "docker node inspect <node ID>"        # Inspect a node
docker-machine ssh myvm1 "docker swarm join-token -q worker"   # View join token
docker-machine ssh myvm1   # Open an SSH session with the VM; type "exit" to end
docker node ls                # View nodes in swarm (while logged on to manager)
docker-machine ssh myvm2 "docker swarm leave"  # Make the worker leave the swarm
docker-machine ssh myvm1 "docker swarm leave -f" # Make master leave, kill swarm
docker-machine ls # list VMs, asterisk shows which VM this shell is talking to
docker-machine start myvm1            # Start a VM that is currently not running
docker-machine env myvm1      # show environment variables and command for myvm1
eval $(docker-machine env myvm1)         # Mac command to connect shell to myvm1
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression   # Windows command to connect shell to myvm1
docker stack deploy -c <file> <app>  # Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file
docker-machine scp docker-compose.yml myvm1:~ # Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"   # Deploy an app using ssh (you must have first copied the Compose file to myvm1)
eval $(docker-machine env -u)     # Disconnect shell from VMs, use native docker
docker-machine stop $(docker-machine ls -q)               # Stop all running VMs
docker-machine rm $(docker-machine ls -q) # Delete all VMs and their disk images
```
