# Jenkins CI Example
This repository showcases a CI-Pipeline with Jenkins for the
spring-petclinic demo project. You can find the original source code
at  https://github.com/spring-projects/spring-petclinic

## What to expect
In this example we will cover
1) How to setup a Jenkins Server declaratively
2) How to define a CI Pipeline for a Java Spring Application
3) How to build and publish a Java Application to a Docker Registry

## Prerequisites
We run Jenkins as Docker-compatible container. This gives us great flexibility
as to where we can execute this example. As long as you have a **working 
installation of Docker** and **unrestricted access to the internet**, you 
should not experience any issues when following along. If you do not have
Docker installed on your system, please refer to the official installation
manual at https://docs.docker.com/engine/install/ and make sure, the Docker
daemon is running.

```
$ docker version
Client: Docker Engine - Community
 Version:           20.10.17
 API version:       1.41
 Go version:        go1.17.11
 Git commit:        100c701
 Built:             Mon Jun  6 23:03:17 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.11
  Git commit:       a89b842
  Built:            Mon Jun  6 23:01:23 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.6
  GitCommit:        10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1
 runc:
  Version:          1.1.2
  GitCommit:        v1.1.2-0-ga916309
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

I tested this guide on a fresh installation of Debian Linux 11
and on my Fedora 34 Laptop on 2022-07-10. You might need small
adaptions to get it running on Windows or MacOS.

## Setup
The docker image containing the spring-petclinic web application
will be published to Artifactory. You can get started with Artifactory
for free by creating a Cloud instance at https://jfrog.com/pricing/.
in order for this build step su succeed, you need to provide credentials.
You can use you username and password. However, you should rather use
a dedicated user together with an accessToken. You should also make sure,
that these credentials are handled with due care.
You can create a token on you profile page (top right menu > Edit Profile)
by clicking on Generate an Identity Token after re-authenticating.

To get the Jenkins Server up and running, you just need to clone
this repository, build the custom jenkins image and run it:
```
$ git clone https://github.com/saschascherrer/jenkins-ci-example.git
$ cd jenkins-ci-example/jenkins-server
$ sudo docker build -t jenkins:casc
$ sudo docker run \
  --privileged \
  --detach \
  --name jenkins \
  --env JENKINS_ADMIN_PASSWORD=replaceme \
  --env JENKINS_READER_PASSWORD=changeme \
  --env JFROG_ACCOUNT_USER=MYUSER \
  --env JFROG_ACCOUNT_PASSWORD=accessTokenForMYUSER \
  --publish 8080:8080 \
  jenkins:casc
```
Here you need to replace `MYUSER` with your artifactory user's ID (the
same you use to login to the WebUI) and `accessTokenForMYUSER` with
the Token generated before or with you artifactory usere's password, although
the latter is not recommended.

You can set `JENKINS_ADMIN_PASSWORD` and `JENKINS_READER_PASSWORD` to your
linking. Just keep in mind, that the passwords should be reasonably good, so
make them long enough and include letters, numbers and symbols.

## Access Jenkins
After completing the setup as described above, you can access the Jenkins UI
at http://localhost:8080. The preconfigured users are `admin` and `reader`.
Their passwords were set through environment variables when you started the 
`jenkins:casc` image.

The CI-pipeline is set up to read the pipeline configuration from the 
Jenkinsfile in jenkins-pipeline/Jenkinsfile at the Git-Repository
github.com/saschascherrer/spring-petclinic.
Jenkins lools for new commits on the main branch of this repository
every 15 minutes. You can also trigger a build manually aptly named
button after you chose the `spring-petclinic` project.

## Run the resulting image
To run the image built by Jenkins, the easiest way is to pull it
from the Docker registry and run it:
```
$ docker run \ 
  --rm \
  --detached \
  --name petclinic \
  --publish 9000:8080 \
  saschascherrer.jfrog.io/default-docker-virtual/spring-petclinic:latest
```
You can access the webapplication at http://localhost:9000. If you are
done looking around, you can terminate the container by executing
```
docker stop petclinic
```
The container will be automatically deleted, because we specified the
`--rm` flag in the `docker run` command.

If you do not want to use a registry, you can grab the `docker-image.tar`
from the Jenkins build and download it to your Docker host maschine.
There you can load and run the image by executing
```
$ sudo docker load -i docker-image.tar 
3f3b1121d064: Loading layer [==================================================>]  53.17MB/53.17MB
Loaded image ID: sha256:d308d93fcbf9ee144ea84635093b324770c8ea6b164108c0b79812159095897e
$ sudo docker run --rm -d --name petclinic --publish 9000:8080 d308d93fcbf9ee144ea84635093b324770c8ea6b164108c0b79812159095897e
```
The image ID will likely differ on your system, so adapt the run command accordingly.
