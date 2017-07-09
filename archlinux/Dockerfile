FROM base/archlinux
MAINTAINER Nick Østergaard <oe.nick at gmail dot com>

# Install the kicad dependencies
RUN pacman -Sy --noconfirm archlinux-keyring
RUN pacman -Syu --noconfirm
RUN pacman-db-upgrade
RUN pacman -S --noconfirm ca-certificates ca-certificates-cacert
RUN pacman -S --noconfirm jre8-openjdk-headless
RUN pacman -S --noconfirm base-devel
RUN pacman -S --noconfirm \
    glu \
    hicolor-icon-theme \
    desktop-file-utils \
    swig \
    wxpython \
    cmake \
    glew \
    bzr \
    git \
    glm \
    boost \
    zlib \
    openssl
RUN pacman -Sy --noconfirm python-matplotlib

# Install a basic SSH server
RUN pacman -S openssh --noconfirm
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

# Clean downloaded packages
RUN yes|pacman -Scc

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
#RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=500
#RUN cd kicad-source-mirror && git checkout da9ca2def28317e15a129b5a3fb060a6264f5b5e
#RUN cd kicad-source-mirror && patch -p1 -i ../0001-Enable-C-11.patch

# Do stuff
#CMD mkdir -p build && cd build && cmake .. && make
#RUN mkdir -p kicad-source-mirror/build
#RUN cd kicad-source-mirror/build && cmake -DKICAD_SCRIPTING=ON -DKICAD_SCRIPTING_MODULES=ON -DKICAD_SCRIPTING_WXPYTHON=ON .. && VERBOSE=1 make

# Fix sshd issue
RUN /usr/bin/ssh-keygen -A

# Standard SSH port
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"] 

