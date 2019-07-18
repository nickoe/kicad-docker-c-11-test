FROM ubuntu:16.04

# Install deps
RUN apt-get update -q && \
    apt-get install --no-install-recommends --no-upgrade -qqy cmake build-essential g++ libwxgtk3.0-dev libglew-dev libglm-dev libcurl3 libcurl4-gnutls-dev libcairo2-dev libboost-dev libboost-test-dev libboost-filesystem-dev libboost-system-dev libboost-regex-dev git autoconf libtool automake  flex bison liboce-foundation-dev liboce-ocaf-dev swig python python-dev ca-certificates python-wxgtk3.0 python-wxgtk3.0-dev doxygen graphviz libssl-dev ccache curl && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Compile and install libngspice
RUN git clone git://git.code.sf.net/p/ngspice/ngspice && \
    cd ngspice && \
    git checkout ngspice-30-2 && \
    ./autogen.sh && \
    ./configure --with-ngshared --enable-xspice --enable-cider --enable-openmp && \
    make -j$(nproc) && \
    make install && \
    cd ..  && \
    rm -rf ngspice /tmp/* /var/tmp/*

# Download and install linuxdeploy tool
RUN curl -O -J -L https://github.com/linuxdeploy/linuxdeploy/releases/download/continuous/linuxdeploy-x86_64.AppImage && \
    chmod +x linuxdeploy-x86_64.AppImage && \
    ./linuxdeploy-x86_64.AppImage --appimage-extract && \
    mv squashfs-root /usr/lib/linuxdeploy && \
    ln -s /usr/lib/linuxdeploy/AppRun /bin/linuxdeploy && \
    chmod -R 775 /usr/lib/linuxdeploy/ && \
    rm -rf *.AppImage /tmp/* /var/tmp/*

# Install JDK 8 for jenkins
RUN apt-get update -q
RUN apt search openjdk
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

WORKDIR /home/jenkins
ENTRYPOINT ["/usr/local/bin/run-ci-worker"]
