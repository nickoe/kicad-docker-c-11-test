# This is to test building kicad on oldest supported Ubuntu version
# currently. Goal is to see if it works with system boost and C++11.

FROM ubuntu:14.04

WORKDIR /mnt/kicad-test

COPY ./0001-Enable-C-11.patch .

RUN pwd
RUN ls

RUN apt-get update
RUN apt-get -y install git bzr make cmake 

RUN echo foo
RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=1
RUN cd kicad-source-mirror && patch -p1 -i ../0001-Enable-C-11.patch

RUN apt-get -y install build-essential
RUN apt-get -y install libglew-dev wxgtk3.0-dev x3.0-headers libglm-dev libcurl4-openssl-dev python2.7 pkg-config libboost1.54-all-dev swig libcairo2-dev doxygen openssl

#CMD mkdir -p build && cd build && cmake .. && make
RUN mkdir -p kicad-source-mirror/build
RUN cd kicad-source-mirror/build && cmake .. && VERBOSE=1 make

