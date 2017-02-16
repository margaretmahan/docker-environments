# Dockerfile to create docker image of fsl 

# usage: docker run -ti --rm -v <host volume>:<containter volume> fsl_env

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
    
# update, use Tini - an init for containers
RUN apt-get install -y grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean
    
# run configuration script for normal usage
RUN echo ". /etc/fsl/5.0/fsl.sh" >> /root/.bashrc
