# Customized Docker Image for Jenkins
This folder contains everything you need to customize the official 
Jenkins Docker Image from https://hub.docker.com/r/jenkins/jenkins
to suit our needs.


## Added functionality
Due to our needs we customize the official Jenkins Image to 
* use JCasC (Jenkins Configuration as Code)
* install the Jenkins plugins we want
* skipt the Post-Installation-Wizard


## Build and run
You can build the customized image using
```
$ sudo docker build -t jenkins:jcasc .
```
and the start a Jenkins container with
```
$ sudo docker run -d --name jenkins -p 8080:8080 --env JENKINS_ADMIN_PASSWORD=password jenkins:jcasc
```
You certainly want to replace the admin password `password` with some
other password that is not as easily guessable.

Your Jenkins server now runs on https://localhost:8080 and you should be
able to login as user `admin` with the password `password` or whichever
password you chose in the previous step.

To stop the Server and clean up the containers, you can execute
```
$ sudo docker stop jenkins
$ sudo docker rm jenkins
```

## Reference:
DigitalOcean has a great guide on how to declaratively install 
and configure Jenkins which I used to create my own custom config:
https://www.digitalocean.com/community/tutorials/how-to-automate-jenkins-setup-with-docker-and-jenkins-configuration-as-code
