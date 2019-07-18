FROM debian:sid as build-env
MAINTAINER Nick Østergaard <oe.nick at gmail dot com>

# Install the kicad dependencies
RUN apt-get update
RUN apt-get -y install git bzr make cmake
RUN apt-get -y install build-essential
RUN apt-get -y install libglew-dev wxgtk3.0-dev x3.0-headers libglm-dev libcurl4-openssl-dev python2.7 pkg-config libboost-all-dev swig libcairo2-dev doxygen openssl libssl-dev
RUN apt-get -y install ngspice ngspice-dev libngspice0 libngspice0-dev
# Install JDK 7 for jenkins
RUN apt-get install -y openjdk-8-jdk
RUN apt-get install -y curl wget


RUN mkdir -p /opt/app-root/jenkins /home/jenkins && \
    chown -R 1001:0 /opt/app-root/jenkins /home/jenkins && \
    chmod -R g+w /opt/app-root/jenkins /home/jenkins && \
    chmod 664 /etc/passwd && \
    chmod -R 775 /usr/lib/jvm && \
    chmod 775 /usr/bin


# Copy upstream run-jnlp-client script and helpers
ADD https://raw.githubusercontent.com/openshift/jenkins/master/slave-base/contrib/bin/generate_container_user /usr/local/bin/
#ADD https://raw.githubusercontent.com/openshift/jenkins/master/slave-base/contrib/bin/run-jnlp-client /usr/local/bin/
COPY run-jnlp-client /usr/local/bin/
ADD https://raw.githubusercontent.com/openshift/jenkins/master/slave-base/contrib/bin/configure-slave /usr/local/bin/
ADD https://raw.githubusercontent.com/openshift/jenkins/master/slave-base/contrib/bin/configure-agent /usr/local/bin/
#ADD openshift-jenkins-upstream/slave-base/contrib/bin/* /usr/local/bin/

# Copy our own entrypoint script with support for GitLab CI
#COPY contrib/bin/* /usr/local/bin/
ADD https://gitlab.cern.ch/ci-tools/ci-worker/raw/master/ci-base/contrib/bin/run-ci-worker /usr/local/bin/
RUN chmod +rwx /usr/local/bin/*
RUN apt-get install -y desktop-file-utils
RUN apt-get install -y fuse
RUN modprobe fuse
#USER jenkins
#USER 1001
WORKDIR /home/jenkins
ENTRYPOINT ["/usr/local/bin/run-ci-worker"]

