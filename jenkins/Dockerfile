FROM liatrio/jenkins-alpine:2.89

COPY maven-global-settings-files.xml /usr/share/jenkins/ref/maven-global-settings-files.xml
USER root
COPY sonar-runner /var/jenkins_home/sonar
