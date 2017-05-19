FROM jenkinsci/jenkins:2.61-alpine

USER root

RUN apk -U add docker
# Setup Jenkins
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
RUN apk add --update shadow \
    && groupadd -g 50 staff \
    && usermod -a -G staff jenkins
USER jenkins
RUN /usr/local/bin/install-plugins.sh ansicolor \
blueocean \
build-environment \
cloudbees-folder \
config-file-provider \
copy-project-link \
credentials-binding \
credentials \
docker-plugin \
docker-slaves \
envinject \
git \
groovy \
job-dsl \
jobConfigHistory \
naginator \
pam-auth \
workflow-aggregator \
sonar

COPY basic-security.groovy /usr/share/jenkins/ref/init.groovy.d/basic-security.groov
COPY jobs /usr/share/jenkins/ref/jobs/
COPY maven-global-settings-files.xml /usr/share/jenkins/ref/maven-global-settings-files.xml
USER root
COPY sonar-runner /var/jenkins_home/sonar
