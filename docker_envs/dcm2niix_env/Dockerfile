# Dockerfile to create docker image for dcm2niix

# usage: docker run -ti -v <host volume with DICOMs>:<container volume> dcm2niix_env

# start with neurodebian image on Xenial Ubuntu 16.04
FROM neurodebian:xenial

MAINTAINER Margaret Mahan <mahan027@umn.edu>

ARG DEBIAN_FRONTEND=noninteractive

# update, install dcm2niix and pigz
RUN apt-get update && \
    apt-get install -y python3 \
    dcm2niix \
    pigz
