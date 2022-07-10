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
