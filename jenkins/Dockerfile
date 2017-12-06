FROM liatrio/jenkins-alpine:latest

COPY maven-global-settings-files.xml /usr/share/jenkins/ref/maven-global-settings-files.xml
USER root
COPY sonar-runner /var/jenkins_home/sonar
