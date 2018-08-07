# Use Ubuntu Bionic image as base
FROM ubuntu:18.04

# Install deps
RUN apt-get update -q && \
    apt-get install --no-install-recommends --no-upgrade -qy cmake build-essential g++ libwxgtk3.0-gtk3-dev libglew-dev libglm-dev libcurl4 libcurl4-gnutls-dev libcairo2-dev libboost-dev libboost-test-dev git autoconf libtool automake flex bison liboce-foundation-dev liboce-ocaf-dev swig python python-dev ca-certificates python-wxgtk3.0 python-wxgtk3.0-dev doxygen graphviz libssl-dev ccache && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Compile and install libngspice
RUN git clone git://git.code.sf.net/p/ngspice/ngspice && \
    cd ngspice && \
    git checkout 99a20162d5038a328d335d11da69c9eee0549fdc && \
    ./autogen.sh && \
    ./configure --with-ngshared --enable-xspice --enable-cider --enable-openmp && \
    make && \
    make install && \
    cd ..  && \
    rm -rf ngspice /tmp/* /var/tmp/*
