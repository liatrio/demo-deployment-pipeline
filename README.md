# Demo Deployment Pipeline

This project demonstrates a complete software delivery pipeline using Docker containers.

The pipeline includes the following Docker containers:
 - Nginx (hosts the Dashboard)
 - Jenkins (drives the Pipeline)
 - Nexus (artifact repository)
 - SonarQube (code quality)
 - Tomcat (deployment target/host for a [demo](https://github.com/liatrio/spring-petclinic) application)

## Prerequisite
 - [Docker](https://www.docker.com/get-docker)!

## Getting started (instructions for mac)
1. check that Docker engine is up and running
1. open Terminal
1. do ```git clone https://github.com/liatrio/demo-deployment-pipeline.git```
1. cd into directory
1. do ```docker-compose up --build```
1. open [localhost](http://localhost) in your browser

Browsing to [localhost](http://localhost) should display a Dashboard page:
![Dashboard](/dashboard/site/dashboard.png)

How does it work? -
```docker-compose up --build``` will build all the required containers, start them, and run health check against the
Dashboard. Health-check runs a curl command every 15 seconds to verify that a service is up and running. It will mark
the container hosting the dashboard as unhealthy if the curl command fails.

## How Jenkins Drives the Pipeline
Jenkins will be populated with a [spring-petclinic](https://github.com/liatrio/spring-petclinic) pipeline job.  This pipeline job is configured with a [declarative pipeline](https://jenkins.io/doc/book/pipeline/syntax/) "Jenkinsfile" which specifies all of the stages and events of the pipeline.

This pipeline job will run through these stages:
 1. Builds the project and deploys artifacts to Nexus repository server running inside Nexus container. This stage is run
 inside of a container built using ```maven:3.5.0``` image
 1. Deploys the war file to Tomcat server that runs inside Tomcat container. This stage is run inside of a container built
 using a lightweight ```alpine``` image
 1. Executes SonarQube analysis on the project code base by sending data to SonarQube server running inside the SonarQube container.
 This stage is run inside of a container built using ```sebp/sonar-runner``` image. This image provides us with the
 sonar-runner agent  
 * Runs a Selenium test suite against PetClinic UI in headless mode. For this stage Jenkins spins up a container based on
  custom ```liatrio/selenium-firefox``` image. It is based on ```ruby``` image and contain
  gems for selenium and headless, firefox, geckodriver. The project's Dockerfile is available [here](https://github.com/liatrio/selenium-firefox/blob/master/Dockerfile)  
