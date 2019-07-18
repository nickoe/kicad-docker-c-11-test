FROM fedora:26
MAINTAINER Nick Østergaard <oe.nick at gmail dot com>

# Install the kicad dependencies
RUN dnf update -y
RUN dnf -y install git bzr make cmake 
#RUN dnf -y install @development-tools
RUN dnf -y install gcc-c++
RUN dnf -y install desktop-file-utils compat-wxGTK3-gtk2-devel wxPython-devel python-devel swig boost-devel cmake doxygen glew-devel openssl-devel bzip2-devel bzr gettext glm-devel libcurl-devel
#RUN dnf -y install libglew-dev wxgtk3.0-dev wx3.0-headers libglm-dev libcurl4-openssl-dev python2.7 pkg-config libboost-all-dev swig libcairo2-dev doxygen openssl libssl-dev

# Install JDK 8 for jenkins
RUN dnf install -y java-1.8.0-openjdk

# Install a basic SSH server
#RUN dnf install -y openssh-server
#RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
#RUN mkdir -p /var/run/sshd

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
RUN echo foo
RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=500
RUN cd kicad-source-mirror && git checkout da9ca2def28317e15a129b5a3fb060a6264f5b5e^
#RUN cd kicad-source-mirror && patch -p1 -i ../0001-Enable-C-11.patch

# Do stuff
#CMD mkdir -p build && cd build && cmake .. && make
RUN mkdir -p kicad-source-mirror/build
RUN cd kicad-source-mirror/build && cmake -DKICAD_SCRIPTING=ON -DKICAD_SCRIPTING_MODULES=ON -DKICAD_SCRIPTING_WXPYTHON=ON -DKICAD_SPICE=OFF -DKICAD_USE_OCE=OFF .. && make -j4

# Standard SSH port
#EXPOSE 22
#CMD ["/usr/sbin/sshd", "-D"] 

