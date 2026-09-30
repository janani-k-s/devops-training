# Day 3 – Docker Networking

## Overview

Day 3 focused mainly on Docker Networking and practical Docker container networking. We learned about Docker network types, inspected existing networks, created a custom network, connected multiple containers to the same network, tested Docker's internal DNS, and cleaned up the temporary networking resources.

We also practiced bind mounts, Dockerfile creation, building a custom Docker image, running a container from the custom image, port mapping, browser testing, and pushing the Dockerfile work to GitHub as part of the trainer's practical flow.

---

## Docker Network Types

Docker provides different network types for containers.

Common Docker network types:

- bridge
- host
- none

The `bridge` network is commonly used for normal container networking.

The `host` network allows a container to use the host's network stack.

The `none` network disables network connectivity for the container.

---

## List Docker Networks

Command:

docker network ls

This command lists the Docker networks available on the system.

During practice, the following networks were observed:

bridge
host
none
legalens-ai_default

`legalens-ai_default` was an existing Docker Compose network created by the LegalLens project.

---

## Inspect a Docker Network

Command:

docker network inspect bridge

This command displays detailed information about a Docker network, including:

- Network ID
- Network name
- Driver
- Scope
- Subnet
- Gateway
- Connected containers

The default `bridge` network was inspected during the practical session.

The bridge network showed:

Subnet: 172.17.0.0/16
Gateway: 172.17.0.1
Driver: bridge
Scope: local

---

## Create a Custom Docker Network

A custom Docker network was created for networking practice.

Command:

docker network create sjc_demonet

This created a custom network named:

sjc_demonet

---

## Run a Container on the Custom Network

The first Apache container was created and connected to the custom network.

Command:

docker run -d --name apache-network-test --network=sjc_demonet httpd

Command breakdown:

`docker run` creates and starts a container.

`-d` runs the container in detached/background mode.

`--name apache-network-test` assigns a name to the container.

`--network=sjc_demonet` connects the container to the custom network.

`httpd` is the Apache HTTP Server image.

---

## Run a Second Container on the Same Network

A second Apache container was created on the same custom network.

Command:

docker run -d --name apache-network-test-2 --network=sjc_demonet httpd

The two containers were:

apache-network-test

apache-network-test-2

Both containers were connected to:

sjc_demonet

The resulting structure was:

sjc_demonet
|
|-- apache-network-test
|
`-- apache-network-test-2

---

## Inspect the Custom Network

Command:

docker network inspect sjc_demonet

The network inspection confirmed that both Apache containers were connected to the `sjc_demonet` network.

This demonstrated that multiple containers can be connected to the same Docker network.

---

## Container-to-Container Networking

Containers connected to the same Docker network can communicate with each other.

Docker provides internal DNS for containers connected to the same network.

This allows one container to resolve another container using its container name instead of directly using its IP address.

For example:

apache-network-test-2

was able to resolve:

apache-network-test

---

## Docker Internal DNS Test

The following command was used to test Docker's internal DNS:

docker exec apache-network-test-2 getent hosts apache-network-test

The command successfully resolved `apache-network-test` to its container IP address.

This demonstrated Docker's internal DNS and container-name resolution.

---

## Testing with ping

We also attempted to test the connection using:

docker exec -it apache-network-test-2 /bin/bash

After entering the container, the following command was attempted:

ping apache-network-test

The result was:

bash: ping: command not found

This was not a Docker networking failure.

The `httpd` image does not contain the `ping` utility by default.

Therefore, the networking functionality was tested successfully using Docker's internal DNS with:

docker exec apache-network-test-2 getent hosts apache-network-test

---

## Testing with curl

We also attempted:

docker exec apache-network-test-2 curl http://apache-network-test

The `httpd` image did not contain the `curl` command by default.

Therefore, `curl` could not be used for the test.

The successful DNS test using `getent hosts` confirmed that Docker's internal name resolution was working.

---

## Remove the Networking Test Containers

After completing the networking practice, the temporary containers were removed.

Commands:

docker rm -f apache-network-test

docker rm -f apache-network-test-2

The `-f` option forces removal of the containers.

---

## Remove the Custom Network

After removing the containers, the temporary custom network was removed.

Command:

docker network rm sjc_demonet

The command returned:

sjc_demonet

The custom networking practice was then cleaned up.

---

## Bind Mounts

A bind mount was also practiced as part of the Docker hands-on training.

A bind mount maps a directory on the host machine to a directory inside a Docker container.

The mapping used during practice was:

D:\docker-data

to:

/usr/local/apache2/htdocs

The command used was:

docker run -d --name volume-test -v D:\docker-data:/usr/local/apache2/htdocs httpd

Here:

`-v` is used to create a volume/bind mount mapping.

The mapping was:

Windows host:
D:\docker-data

Container:
/usr/local/apache2/htdocs

---

## Verify the Bind Mount

The mounted directory inside the container was checked using:

docker exec volume-test ls /usr/local/apache2/htdocs

Initially, the directory was empty because the host directory was empty.

An `index.html` file was then created in:

D:\docker-data

The file contained:

<h1>Hello from D drive!</h1>

The file was then accessed from inside the container using:

docker exec volume-test cat /usr/local/apache2/htdocs/index.html

The content was successfully displayed inside the container.

This demonstrated that the host directory was successfully mapped to the container directory.

The temporary container was then removed using:

docker rm -f volume-test

The temporary `D:\docker-data` folder was later removed during cleanup.

---

## Dockerfile

A Dockerfile was also practiced during the Docker hands-on training.

A Dockerfile is a text file containing instructions used to build a Docker image.

The Dockerfile used during practice was:

FROM httpd:latest

COPY index.html /usr/local/apache2/htdocs/

EXPOSE 80

---

## Dockerfile Instructions

`FROM httpd:latest`

Specifies the Apache HTTP Server image as the base image.

`COPY index.html /usr/local/apache2/htdocs/`

Copies the local `index.html` file into the Apache web directory inside the image.

`EXPOSE 80`

Documents that the application inside the container uses port 80.

---

## HTML File

The following HTML file was created:

<h1>Hello from my Dockerfile!</h1>

The Dockerfile copied this file into Apache's web directory.

---

## Build a Custom Docker Image

The Docker image was built using:

docker build -t mydockerfile:v1 .

The command created the custom image:

mydockerfile:v1

Command breakdown:

`docker build` builds an image using the Dockerfile.

`-t mydockerfile:v1` assigns the image name and tag.

`.` specifies the current directory as the Docker build context.

---

## Verify the Custom Image

Command:

docker images

The custom image was verified:

mydockerfile:v1

---

## Run a Container from the Custom Image

A container was created from the custom image using:

docker run -d --name dockerfile-test -p 8082:80 mydockerfile:v1

The port mapping was:

Host port: 8082

Container port: 80

Therefore:

8082 -> 80

The host port was used to access the Apache application running inside the container.

---

## Verify the Dockerfile Container

Command:

docker ps -a

The container was verified with:

Container name: dockerfile-test

Image: mydockerfile:v1

Port mapping: 8082 -> 80

The container was running successfully.

---

## Test the Dockerfile Application

The application was opened in a web browser using:

http://localhost:8082

The following message was displayed:

Hello from my Dockerfile!

This confirmed that:

- The Dockerfile was built successfully.
- The custom Docker image was created.
- The container was created successfully.
- Apache was running inside the container.
- Port mapping was working.
- The custom HTML page was served successfully.

---

## Git and GitHub

The Dockerfile and `index.html` were added to the DevOps GitHub repository.

Repository location:

D:\DevOps\devops-training

The Dockerfile work was committed using:

git status

git add .

git commit -m "feat: add Dockerfile practice"

git push

The Dockerfile work was successfully pushed to GitHub.

The files were later organized into the Day 4 Docker Compose folder:

Day-4
|
`-- Docker-Compose
    |
    |-- Dockerfile
    `-- index.html

The old combined `Day-3,4` folder was removed.

---

## Important Commands Practiced

Docker Networking:

docker network ls

docker network inspect bridge

docker network create sjc_demonet

docker network inspect sjc_demonet

docker network rm sjc_demonet

Container Networking:

docker run -d --name apache-network-test --network=sjc_demonet httpd

docker run -d --name apache-network-test-2 --network=sjc_demonet httpd

docker exec -it apache-network-test-2 /bin/bash

docker exec apache-network-test-2 getent hosts apache-network-test

docker rm -f apache-network-test

docker rm -f apache-network-test-2

Bind Mount:

docker run -d --name volume-test -v D:\docker-data:/usr/local/apache2/htdocs httpd

docker exec volume-test ls /usr/local/apache2/htdocs

docker exec volume-test cat /usr/local/apache2/htdocs/index.html

docker rm -f volume-test

Dockerfile:

docker build -t mydockerfile:v1 .

docker images

docker run -d --name dockerfile-test -p 8082:80 mydockerfile:v1

docker ps -a

docker rm -f dockerfile-test

Git:

git status

git add .

git commit -m "feat: add Dockerfile practice"

git push

---

## Key Learning

Docker Networking allows containers to communicate with each other.

The `bridge` network is a commonly used Docker network.

Custom networks can be created using:

docker network create <network-name>

Containers can be connected to a custom network using:

--network=<network-name>

Containers on the same Docker network can resolve each other using container names through Docker's internal DNS.

Bind mounts allow a host directory to be mapped to a directory inside a container.

A Dockerfile contains instructions used to build a Docker image.

A Docker image can be created using:

docker build

A container can then be created from the custom image using:

docker run

Port mapping connects a host port to a container port.

Example:

8082:80

means:

Host port 8082 -> Container port 80

---

## Day 3 Completion

The following practical topics were completed:

- Docker network types
- Listing Docker networks
- Inspecting Docker networks
- Creating a custom Docker network
- Connecting containers to a custom network
- Running multiple containers on the same network
- Docker internal DNS
- Container-name resolution
- Removing networking containers
- Removing a custom network
- Bind mounts
- Dockerfile creation
- Building a custom Docker image
- Running a container from a custom image
- Port mapping
- Testing the application in a browser
- Git commit
- Git push

The primary focus of Day 3 was Docker Networking.

The next training day will focus on Docker Compose and multi-container applications.