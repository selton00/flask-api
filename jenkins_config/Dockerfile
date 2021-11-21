FROM jenkins/jenkins:2.321-jdk11
USER root
RUN curl -fsSL https://get.docker.com/ | sh
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false
ENV CASC_JENKINS_CONFIG /var/jenkins_home/JCasC.yaml
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
COPY JCasC.yaml /var/jenkins_home/JCasC.yaml