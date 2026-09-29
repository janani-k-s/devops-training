# Day 2 – Docker

## Docker Fundamentals

Docker is a platform used to build, run and manage applications inside containers.

An image is a template used to create containers.

A container is an instance created from an image.

One image can be used to create multiple containers.

## Docker Images

Docker images can be searched and downloaded from Docker Hub.

Commands practiced:

docker search httpd
docker pull httpd
docker images

`httpd` is the official Apache HTTP Server image.

## Docker Containers

Container creation and execution were practiced using the Apache `httpd` image.

Commands practiced:

docker create --name apache-container httpd

docker start apache-container

docker ps

docker ps -a

`docker create` creates a container but does not start it.

`docker start` starts an existing stopped container.

`docker run` creates and starts a container in a single command.

Example:

docker run -d --name apache-container httpd

The `-d` option runs the container in detached/background mode.

## Docker Container Lifecycle

The following container lifecycle commands were practiced:

docker stop apache-container

docker start apache-container

docker rm apache-container

docker rm -f temp-container

`docker stop` stops a running container.

`docker start` starts a stopped container.

`docker rm` removes a stopped container.

`docker rm -f` forcefully stops and removes a container.

## Accessing a Running Container

A running Apache container was accessed using:

docker exec -it apache-container /bin/bash

`docker exec` is used to execute a command inside a running container.

The Apache web directory was explored:

cd /usr/local/apache2/htdocs

The default `index.html` file was viewed using:

cat index.html

The HTML file was modified from inside the container.

## Docker Commit

After modifying the container, a new Docker image was created from the container using:

docker commit apache-container customimg

This creates the image:

customimg:latest

The difference between an image and a container was practiced.

Image = Template / Blueprint

Container = Instance created from an image

## Docker Save

The custom Docker image was exported as a `.tar` file using:

docker save -o customimg.tar customimg:latest

This converts:

Docker Image → .tar file

The generated `customimg.tar` file was verified on the system.

## Docker Load

The custom image was removed and restored from the `.tar` file using:

docker load -i customimg.tar

This converts:

.tar file → Docker Image

The restored image was verified using:

docker images

## Docker Logs

Container logs were viewed using:

docker logs log-demo

Logs can be followed continuously using:

docker logs -f log-demo

`docker logs` is useful for checking application output and troubleshooting containers.

## Docker Inspect

Detailed information about a container was viewed using:

docker inspect log-demo

`docker inspect` provides detailed configuration information such as container settings, networking information, mounts, IP address and other metadata.

## Docker Port Mapping

Port mapping was introduced using the `-p` option.

Example:

docker run -d --name apache-container -p 8081:80 httpd

Here:

8081 = Host port

80 = Container port

The mapping can be represented as:

Host port 8081 → Container port 80 → Apache

The host port is the port exposed on the Windows machine, while the container port is the port on which the application listens inside the container.

## Docker Cleanup

Docker cleanup commands were discussed.

docker system prune

docker system prune -a

These commands can remove unused Docker resources. They should be used carefully, especially in company or production environments, because important unused resources may be removed.

## Commands Practiced

docker -v

docker search

docker pull

docker images

docker create

docker run

docker ps

docker ps -a

docker start

docker stop

docker exec

docker logs

docker inspect

docker rm

docker rm -f

docker commit

docker save

docker load

## Key Learning

Docker Image
→ Template used to create containers

Docker Container
→ Running or created instance of an image

docker run
→ Creates and starts a container

docker exec
→ Executes commands inside a running container

docker commit
→ Creates an image from a container

docker save
→ Saves an image as a `.tar` file

docker load
→ Restores an image from a `.tar` file

docker logs
→ Displays container/application logs

docker inspect
→ Displays detailed container information

Port Mapping
→ Connects a host port to a container port