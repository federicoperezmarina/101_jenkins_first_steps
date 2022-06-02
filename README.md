# 101_jenkins_first_steps
This repository is about how to use jenkins

## Table of Contents
1. [Dockerfile](#dockerfile)
2. [Build the image from Dockerfile](#build-the-image-from-dockerfile)
3. [Run the image](#run-the-image)
4. [Execute the image](#execute-the-image)
5. [Start Jenkins Installation](#start-jenkins-installation)
6. [Create a new task](#create-a-new-task)

## Dockerfile
This is the Dockerfile that we want to use to build the image of jenkins.
```sh
FROM jenkins/jenkins:2.332.3-jdk11
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
docker build -t myjenkins-blueocean:2.332.3 .
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
  myjenkins-blueocean:2.332.3 
```

## Execute the image
In the fourth step we want to show how to enter in the container with a simple docker command
```sh
docker exec -it jenkins-blueocean bash
```
## Start Jenkins Installation
To start the installation we should go to:
```sh
http://localhost:8080
```

In the browser we will see this image the first time.
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/unlock_jenkins.png" width="400px"/></p>

To get the initial password do
```sh
$ docker ps
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS          PORTS                                              NAMES
c7b0e756ca7f   myjenkins-blueocean:2.332.3   "/sbin/tini -- /usr/…"   16 minutes ago   Up 16 minutes   0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins-blueocean
$ docker logs <container_id>
$ docker logs c7b0e756ca7f
or
$ cat /var/jenkins_home/secrets/initialAdminPassword
```

In this step we see something like that:
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/wellcome_jenkins.png" width="400px"/></p>

Now we are able to create an administrator for the jenkins:
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/admin_jenkins.png" width="400px"/></p>

We will complete the info for the jenkins administrator and restart the jenkins.
```sh
user:admin
pass:admin
repass:admin
administrator
your_email@domain.com
```

If we are able to see this login, we have done a good job!
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/login_jenkins.png" width="400px"/></p>

## Create a new task
Step 1: we are going to create a new task (pipeline):
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/new_task_jenkins.png"/></p>

Step 2: we have to write the name of the task 'my-pipeline' and select 'Multibranch pipeline'
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/new_task_2_jenkins.png"/></p>

Step 3: we have to introduce the name 'my-pipeline', add the github repository 'https://github.com/federicoperezmarina/101_jenkins_first_steps' and save it.
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/new_task_3_jenkins.png"/></p>

Step 4: we will list all the pipelines going to the next url 'http://localhost:8080/'
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/pipelines_jenkins.png"/></p>

Step 6: we select 'my-pipeline'
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/my-pipeline_jenkins.png"/></p>

Step 7: we can see now all the stages in the pipeline 'my-pipeline'
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/stage_view_jenkins.png"/></p>

Step 8: we click into console output to see how the pipelines has been executed
<p><img src="https://github.com/federicoperezmarina/101_jenkins_first_steps/blob/main/img/console_output_jenkins.png"/></p>
