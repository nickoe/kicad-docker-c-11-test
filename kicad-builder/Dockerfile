# Use Ubuntu Bionic image as base
FROM ubuntu:16.04

# Install deps
RUN apt-get update -q && \
    apt-get install --no-install-recommends --no-upgrade -qqy cmake build-essential g++ libwxgtk3.0-dev libglew-dev libglm-dev libcurl3 libcurl4-gnutls-dev libcairo2-dev libboost-dev libboost-test-dev git autoconf libtool automake  flex bison liboce-foundation-dev liboce-ocaf-dev swig python python-dev ca-certificates python-wxgtk3.0 python-wxgtk3.0-dev doxygen graphviz libssl-dev ccache curl && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Compile and install libngspice
RUN git clone git://git.code.sf.net/p/ngspice/ngspice && \
    cd ngspice && \
    git checkout 99a20162d5038a328d335d11da69c9eee0549fdc && \
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
    rm -rf *.AppImage /tmp/* /var/tmp/*

