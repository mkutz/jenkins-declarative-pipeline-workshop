# Jenkins Declarative Pipeline Workshop

In this workshop we will create Jenkinsfiles for Java projects. Those file describe how a software should be build by Jenkins. Basically we will go handson through the [Pipeline Syntax documentation].

## Prerequisites

* Docker
* GitHub account (or any publicly accessible Git repository)
* Internet connection

## Setup

1. Get your local Jenkins up and running using [this Docker image](https://hub.docker.com/r/jenkins/jenkins/):
   ```bash
   docker run --name my-jenkins -p 8080:8080 -p 50000:50000 \
     -v jenkins_home:/var/jenkins_home \
     -v /var/run/docker.sock:/var/run/docker.sock \
     jenkins/jenkins:latest
   ```
   This will download Jenkins form Docker Hub, start it and bind its ports `8080` and `50000` to the same ports on your local machine. It will also persist the Jenkins configuration in a Docker volume named `jenkins_home`, so it survives when you shutdown the container.
2. Copy the initial admin password from the console output.
3. Open your [local Jenkins], enter the password and "install suggested plugins". This will take a while.
4. While plugins get installed, create a new Git repository for your Jenkinsfile (e.g. on GitHub, GitLab or BitBucket).
5. Enter you account data.

## Basics

Read the chapter about [Sections chapter] to learn about `agent`, `post`, `stages` and `steps`.

### Sections

-[ ] In you Git repository create a file named `Jenkinsfile.helloworld`.
-[ ] Create a simple hello world pipeline with one `stage` to print "Hello World", which runs on any agent.
-[ ] Commit and push the file.
-[ ] Go to your [local Jenkins] and click on "*New Item*", enter the name "`hello-wold`". Select "*Pipeline*" and hit *OK*.\
  The name will be used for the job's URL, so using upper case, whitespace or special characters is not advisable. It will work –using URL encoding– but you'll end up with ugly URL's.
-[ ] Find the "Pipeline" section and change the definition to "Pipeline script from SCM", enter you Git repositories URL (don't use SSH but HTTPS, so we don't need to setup credentials) and set the "Script Path" to "`Jenkinsfile.helloworld`". Hit "Save".
-[ ] Execute your pipeline by hitting "Build Now".
-[ ] Once the job has finished, navigate to it and check the *Console Output* to contain the "Hello World" output.
-[ ] Add a `post` section to your stage to always echo "Bye World". Save, commit, push and execute the pipeline again. Check the results.   

### Directives

Read the [Directives chapter] to learn about the possible directives. Especially parameters and options.

-[ ] Use and `environment` variable `GREETING` to print your name instead of "World".
-[ ] Use another `environment` variable `GIT_VERSION` set to the latest Git commit message (use an `sh` step in `environment`).\
  Hint: `git log -1 --pretty=%B`
-[ ] Use an `option` to add timestamps to your output.
-[ ] Define a string `parameter` to replace the `GREETING` environment variable. Set "World" as default value.
-[ ] Change the `GREETING` parameter to a choice parameter to choose between "World", "Universe" and "Folks".
-[ ] Configure a `trigger`, so the job gets executed on changes in your repository, polling every minute.\
  We are not using any push mechanism since your Jenkins is local and GitHub won't be able to reach it.
-[ ] Setup a Maven installation in Jenkins UI at "Manage Jenkins" → "Global Tool Configuration" ↴ "Maven Configuration". Name it "maven", check "Install automatically" and "Install from Apache". Now add a `tools` directive to your Jenkinsfile in order to utilize you "maven" installation and add another step executing `mvn --version`. Check for the output.
-[ ] Add an `input` directive to your stage, so it is only run, if you give your permission.
-[ ] Add a `when` directive to your stage, which prevents the stage to be executed if the current `changeset` did not change the Jenkinsfile.\
  Note that several `when` conditions only work for certain types of pipelines. For example the `branch` condition won't work here for we are not using a multi-branch pipeline!

## Multi-Branch Pipelines

Let's use what we learned to actually build something.

-[ ] Head over to [Spring Boot Initializr] and download a simple Spring Boot project with Maven; unpack it into your repository and try to build it locally with `mvn clean install`. Push the files.
-[ ] Create new `Jenkinsfile` to build the new Spring Boot service:\
  Create a stage, which executes `mvn package` and `stash`es the resulting JAR file. Add a `post` stage to collect the `junit` test results.
-[ ] Add a boolean parameter `RELEASE` to your pipeline.
-[ ] Create a stage, which executes `mvn release:prepare release:perform`. The stage should only be executed `when` the `RELEASE` parameter is `true` _and_ the current branch is `master`.


[Pipeline Syntax documentation]: <https://jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline>
[local Jenkins]: <http://localhost:8080>
[Sections chapter]: <https://jenkins.io/doc/book/pipeline/syntax/#declarative-sections>
[Directives chapter]: <https://jenkins.io/doc/book/pipeline/syntax/#declarative-directives>
[Spring Boot Initializr]: https://start.spring.io/