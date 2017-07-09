FROM debian:jessie
MAINTAINER Nick Østergaard <oe.nick at gmail dot com>

# Install the kicad dependencies
RUN apt-get update
RUN apt-get -y install git bzr make cmake 
RUN apt-get -y install build-essential
RUN apt-get -y install libglew-dev wxgtk3.0-dev x3.0-headers libglm-dev libcurl4-openssl-dev python2.7 pkg-config libboost-all-dev swig libcairo2-dev doxygen openssl libssl-dev

# Install JDK 7 for jenkins
RUN apt-get install -y openjdk-7-jdk

# Install a basic SSH server
RUN apt-get install -y openssh-server
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

# Prepare a jenkins user and use it
#RUN useradd -ms /bin/bash jenkins

# Set user jenkins to the image
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins &&\
    echo "jenkins:jenkins" | chpasswd
#USER jenkins
WORKDIR /home/jenkins

# Add a patch file
COPY ./0001-Enable-C-11.patch .

# Prepare workspace
#RUN echo foo
#RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=1
#RUN cd kicad-source-mirror
#CMD mkdir -p build && cd build && cmake .. && make
#RUN mkdir -p kicad-source-mirror/build
#RUN cd kicad-source-mirror/build && cmake -DKICAD_SCRIPTING=ON -DKICAD_SCRIPTING_MODULES=ON -DKICAD_SCRIPTING_WXPYTHON=ON .. && VERBOSE=1 make

# Standard SSH port
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"] 

