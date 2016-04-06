# This is to test building kicad on oldest supported Ubuntu version
# currently. Goal is to see if it works with system boost and C++11.

FROM ubuntu:14.04

WORKDIR /mnt/kicad-test

COPY ./0001-Enable-C-11.patch .

RUN pwd
RUN ls

RUN apt-get update
RUN apt-get -y install git bzr make cmake 
RUN apt-get -y install build-essential
RUN apt-get -y install libglew-dev wxgtk3.0-dev x3.0-headers libglm-dev libcurl4-openssl-dev python2.7 pkg-config libboost1.54-all-dev swig libcairo2-dev doxygen openssl
RUN apt-get -y install software-properties-common
RUN add-apt-repository -y ppa:adamwolf/kicad-trusty-backports
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get -y install python-wxgtk3.0

RUN echo foo
RUN git clone https://github.com/KiCad/kicad-source-mirror --depth=500
RUN cd kicad-source-mirror && git checkout da9ca2def28317e15a129b5a3fb060a6264f5b5e
RUN cd kicad-source-mirror && patch -p1 -i ../0001-Enable-C-11.patch


#CMD mkdir -p build && cd build && cmake .. && make
RUN mkdir -p kicad-source-mirror/build
RUN cd kicad-source-mirror/build && cmake -DKICAD_SCRIPTING=ON -DKICAD_SCRIPTING_MODULES=ON -DKICAD_SCRIPTING_WXPYTHON=ON .. && VERBOSE=1 make

