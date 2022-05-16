# 101_jenkins_first_steps
This repository is about how to use jenkins

## Table of Contents
1. [Dockerfile](#dockerfile)
2. [Build the image from Dockerfile](#build-the-image-from-dockerfile)
3. [Run the image](#run-the-image)
4. [Execute the image](#execute-the-image)

## Dockerfile
This is the Dockerfile that we want to use to build the image of jenkins.
```sh
FROM jenkins/jenkins:2.332.1-jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.3 docker-workflow:1.28"
```

## Build the image from Dockerfile
In the second step we show how to build the image having the dockerfile
```sh
docker build -t myjenkins-blueocean:2.332.1-1 .
```
## Run the image
In the third step we are going to run the the docker build image
```sh
docker run \
  --name jenkins-blueocean \
  --rm \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.1-1 
```

## Execute the image
In the fourth step we want to show how to enter in the container with a simple docker command
```sh
docker exec -it jenkins-blueocean bash
```
