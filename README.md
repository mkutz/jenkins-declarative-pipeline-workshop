# Jenkins Declarative Pipeline Workshop

In this workshop we will create Jenkinsfiles for Java projects. Those file describe how a software should be build by Jenkins. Basically we will go handson through the [Pipline Syntax documentation].

## Prerequisites

* Docker
* GitHub account (or any publicly accessable Git repository)
* Internet connection

## Setup

1. Get your local Jenkins up and running using [this Docker image](https://hub.docker.com/r/jenkins/jenkins/):
   ```bash
   docker run --name myjenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:latest
   ```
   This will download Jenkins form Docker Hub, start it and bind its ports `8080` and `50000` to the same ports on your local machine. It will also persist the Jenkins configuation in a Docker volume named `jenkins_home`, so it survives when you shutdown the container.
2. Copy the initial admin password from the console putput.
3. Open your [local Jenkins], enter the password and "install suggested plugins". This will take a while.
4. While plugins get installed, create a new Git repository for your Jenkinsfiles (e.g. on GitHub or GitLab).
5. Enter you account data.

[Pipline Syntax documentation]: <https://jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline>
[local Jenkins]: <http://localhost:8080>