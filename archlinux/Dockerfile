FROM base/archlinux
MAINTAINER Nick Østergaard <oe.nick at gmail dot com>

# Install the kicad dependencies
RUN pacman -Sy --noconfirm archlinux-keyring
RUN pacman -Syu --noconfirm
RUN pacman-db-upgrade
RUN pacman -S --noconfirm ca-certificates ca-certificates-cacert
RUN pacman -S --noconfirm jre8-openjdk-headless
RUN pacman -S --noconfirm base-devel
RUN pacman -Sy --noconfirm \
    glu \
    hicolor-icon-theme \
    desktop-file-utils \
    swig \
    wxgtk2 \
    wxpython \
    ngspice \
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
RUN passwd -d jenkins # Delete the buildusers password
RUN printf 'jenkins ALL=(ALL) ALL\n' | tee -a /etc/sudoers # Allow the jenkins passwordless sudo
USER jenkins
WORKDIR /home/jenkins
ENV MAKEFLAGS=-j40
# Stuff for OCC patch
RUN git clone https://aur.archlinux.org/package-query.git && \
    cd package-query && makepkg -si --noconfirm
RUN git clone https://aur.archlinux.org/yaourt.git && \
    cd yaourt && makepkg -si --noconfirm

# Lets install the various OCC's that are available on archlinux right
# now
RUN yaourt -S --noconfirm opencascade
#RUN yaourt -S --noconfirm opencascade7
RUN yaourt -S --noconfirm oce


# Add a patch file
COPY ./0001-Enable-C-11.patch .
COPY ./0001-Allow-Kicad-to-use-OpenCascade.patch .

USER jenkins
# Prepare workspace
RUN echo foo
RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=500
RUN cd kicad-source-mirror

CMD mkdir build_oce && cd build_oce && \
    cmake ../kicad-source-mirror \
    -DKICAD_SCRIPTING_WXPYTHON=OFF \
    -DKICAD_SCRIPTING_MODULES=OFF \
    -DKICAD_SCRIPTING=OFF \
    -DOCE_DIR=/opt/oce/lib/oce-0.18

CMD mkdir build_occt6 && cd build_occt6 && \
    cmake ../kicad-source-mirror \
    -DKICAD_SCRIPTING_MODULES=OFF \
    -DKICAD_SCRIPTING=OFF \
    -DKICAD_SCRIPTING_WXPYTHON=OFF

# Do stuff
#CMD mkdir -p build && cd build && cmake .. && make
#RUN mkdir -p kicad-source-mirror/build
#RUN cd kicad-source-mirror/build && cmake -DKICAD_SCRIPTING=ON -DKICAD_SCRIPTING_MODULES=ON -DKICAD_SCRIPTING_WXPYTHON=ON .. && VERBOSE=1 make

# We need root here
USER root

# Fix sshd issue
RUN /usr/bin/ssh-keygen -A

# Standard SSH port
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"] 

