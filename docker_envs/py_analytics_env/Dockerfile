# Dockerfile to create docker image for anaconda with additional requirements.txt incorporated
# requirements.txt includes:
#	pydicom==0.9.9
#	selenium==3.0.2

# usage: docker run -ti --rm -v <host volume>:<containter volume> py_analytics_env
# usage with jupyter notebook: docker run -i -t -p 8888:8888 py_analytics_env /bin/bash
#       -c "/opt/conda/bin/conda install jupyter -y --quiet && mkdir /opt/notebooks
#       && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip='*' --port=8888 --no-browser"

# start with image from Xenial Ubuntu 16.04
FROM ubuntu:xenial

MAINTAINER Margaret Mahan <mahan027@umn.edu>

# update, install wget, bzip2, and curl
RUN apt-get update && \
    apt-get install -y wget \
    bzip2 \
    curl

# get anaconda
RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda3-4.3.0-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

# update, use Tini - an init for containers
RUN apt-get install -y grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean
    
# configure environment
ENV PATH /opt/conda/bin:$PATH
CMD /bin/bash

# install python requirements using pip which is installed with anaconda on python 3.6
COPY requirements.txt /tmp/
RUN apt-get update && \
    pip install -r /tmp/requirements.txt
