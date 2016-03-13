# This is to test building kicad on oldest supported Debian version
# currently. Goal is to see if it works with system boost and C++11.

FROM debian:jessie

RUN apt-get update
RUN apt-get -y install git bzr make cmake 
RUN apt-get -y install build-essential
RUN apt-get -y install libglew-dev wxgtk3.0-dev x3.0-headers libglm-dev libcurl4-openssl-dev python2.7 pkg-config libboost-all-dev swig libcairo2-dev doxygen openssl libssl-dev

WORKDIR /mnt/kicad-test

RUN echo foo
RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=1
RUN cd kicad-source-mirror
#CMD mkdir -p build && cd build && cmake .. && make
RUN mkdir -p kicad-source-mirror/build
RUN cd kicad-source-mirror/build && cmake .. && make

