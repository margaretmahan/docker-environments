# Dockerfile to create docker image for fsl with gui

# usage: docker run -ti --rm -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME/.Xauthority:/home/developer/.Xauthority -v <host volume>:<container volume> --net=host --pid=host --ipc=host fsl_gui_env
# may need to modify for xhost configuration

# start with image from Xenial Ubuntu 16.04
FROM ubuntu:xenial

MAINTAINER Margaret Mahan <mahan027@umn.edu>

# update, install curl and python
RUN apt-get update && \
    apt-get install -y curl \
    python2.7

# get fsl
RUN curl http://neuro.debian.net/lists/xenial.us-ca.full >> /etc/apt/sources.list.d/neurodebian.sources.list && \
	apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9
	
# update, install fsl
RUN apt-get update && \
    apt-get install -y fsl-complete && \
    rm -rf /var/cache/apt/    
    
# configure environment
ENV DISPLAY :0
ENV USER=developer

# setup for gui
RUN export uid=1000 gid=1000 && \
	mkdir -p /home/developer && \
	mkdir -p /etc/sudoers.d && \
	echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
	echo "developer:x:${uid}:" >> /etc/group && \
	echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
	chmod 0440 /etc/sudoers.d/developer && \
	chown ${uid}:${gid} -R /home/developer

# update, use Tini - an init for containers
RUN apt-get install -y grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

# add more environment settings
USER developer
ENV HOME /home/developer

# run configuration script for normal usage
RUN echo ". /etc/fsl/5.0/fsl.sh" >> /home/developer/.bashrc
