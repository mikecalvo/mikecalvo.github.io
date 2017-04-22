---
title: Web Application Development
layout: default
---
theme: Next, 3

## Docker
### MSSE 2017

---
# Goals
- Gain an understanding of Docker
- Potentially Dockerize your application
- Prepare your application for the cloud

---
# What is Docker
- Project that automates deployment of applications inside software containers
- Layer of abstraction on operating system virtualization
- Docker is not a VM

---
# How it works
- Docker containers have extremely light weight components of an operating system
- The other things they require to run obtained from the host VM
- Can only be run on Linux as Docker takes advantage of cgroups and kernel namespaces to achieve isolation

---
# Benefits
- Extremely lightweight containers that can run virtually anything
- Isolation from other containers on the host machine
- Ability to install from an exact specification
- On demand instantiation of new containers

---

# Running Docker
- Docker used to run inside of a Virtualized Linux image (Docker Machine)
- Docker now has native application support for Windows and Mac
- Install at -> [http://www.docker.com](http://www.docker.com)

---

# Components of Docker
- Docker Engine
- Images
- Containers
- Docker Registry

---

# Docker Engine
- Client CLI
- Rest Layer
- Docker Daemon

---

# Images
- An instance of a application
- Images are immutable
- Ability to start from a pristine, working application
- See docker images `docker images`

---

# Images include
- Code
- Networking
- Configuration
- Dependencies
- Can include a small subset of an operating system

---

# Image properties
- Repository (name)
- Tag (Version - latest by default)
- Image ID
- Size

---

# Image sizes
- Docker reuses data from other containers
- What's listed in images is usually not representative of total container space
- Usually much smaller

---

# Starting a container from a given image
- Docker will look locally for the image name
- If it's not found - will pull from the public docker repository
- If a version is not specified, will run `latest`
- Run a image via `docker run $IMAGE_NAME:$VERSION`
- Starts a container from the given image

---

# Example - Running a image

``` bash
# docker run -ti ubuntu:latest ls

# docker run -ti ubuntu:latest bash

```

---

# Containers
- An interactive instance of the application you started
- Created from images
- Isolated from other processes and containers

---

# Container Commands
- See a list of running containers `docker ps`
- See all created containers `docker ps -a`
- See last container exited `docker ps -l`

---

# Container Naming
- Containers have unique ID
- Container name is generated unless you specify
`--name=example`

---

# Container properties
- Containers do not overwrite images
- When main process exits, containers stop (`-d (detached) to keep running`)
- Stopping a container does not remove or delete it (can specify --rm)

``` groovy
docker run —rm -ti ubuntu sleep 5`
```

---

# Immutability in Docker
- Docker images are immutable
- Docker containers can be "committed into new images"
- This new image will contain your saved work

---

# Docker flow
- New container from an image
- Container is changed in some meaningful way
- Docker commit is run
- A new image is created

---

# Commit example

``` bash
# docker run -it ubuntu bash

//In container
# touch NEW_FILE
# exit
//Outside of container

//Commit the container with a new image name
# docker commit $NAME or $ID new-image

//List the new image
# docker images
```

---

# Exposing ports
- Use `-p` when running a docker to expose a port

``` bash

docker run -p 8081:8080 somecontainer

```
- 8081 is the port you would connect to
- 8080 is the port your connection is redirected to

---

# Let docker determine port
- Docker will pick an available port for you if you don't specify one
- Use `-p` when running a docker to expose a port

``` java

docker run -p 8080 somecontainer

```
- `docker port $CONTAINER_NAME`
- 8080 is the port your connection is redirected to

---

# Docker files
- Files with instructions for creating a docker image
- Each line represents an instruction to build a container
- Each line creates a new image
- Final image is saved in `docker images`

---

# Docker file Commands
- FROM the base origin of the image
  - Generally some kind of system
  - Can be another docker image
- ADD - Add a local file to the docker image
- RUN - Run a command on the system
  - Sometimes used to do updates

---

# Couple more
- ENV - Adds environment variable to the container
- EXPOSE - Exposes the internal port
- ENTRYPOINT - Execute command

---

# Example - Spring Boot Docker File

- `./gradlew build` //Make sure you've build the jar

``` bash

FROM java:alpine
ADD build/libs/week-12-forms-0.0.1-SNAPSHOT.jar app.jar
RUN sh -c 'touch /app.jar'
ENV JAVA_OPTS=""
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" ]

```
---

# Creating the image

``` Java
docker build -t msse/week12forms:1.0 .
```

- `-t` tag name - $NAME:$Version
- `.` < Very important. Location of Dockerfile

---

# Run the container

`docker run --rm -ti -p 8080:8080 msse/week12forms:1.0`

- Starts up the application accessible at port 8080

---

# Attaching to a running container
- `docker exec --ti ubuntu bash`

---

# Docker Networking
- Docker has private networks
- `docker network create $NAME`
- Starts a name server

``` bash
docker run --rm -ti --net=$NAME --name example-server <container>
docker run --rm -ti --net=$NAME --link example-server <container2> --name example-server2
```
- example-server2 now gets network information for example-server one

---
# Docker Compose
- Let's you run many containers at once
- Useful for testing local environment
- Not for production - can't really scale
- Run via `docker-compose up`

---
# Docker Registry
- Repository for images
- Can be private for organizations
- Docker hub - public repository
  - Can create an account and host your own images

---

# Docker and the Cloud
- Docker is generally used with service orchestration
  - Service Discovery
  - Starting/Stopping Containers

---

# Docker reference
- Docker has many many more moving parts
- Docker swarm
- Lots of other features
  - Shared file systems
  - Other networking options
- [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
