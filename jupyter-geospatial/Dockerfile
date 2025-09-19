# OSU-Carpentry Jupyter Geospatial
# This image should have all software needed to support the Data Carpentry Geospatial lessons.

ARG BASE_CONTAINER=osucarpentry/jupyter-base:latest
FROM $BASE_CONTAINER

LABEL maintainer="Evan Linde <elinde@okstate.edu>" \
    description="OSU-Carpentry Jupyter Geospatial"

USER root

# Geospatial dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libudunits2-dev \
    libgdal-dev \
    libgeos-dev \
    libproj-dev \
    && \
    rm -rf /var/lib/apt/lists/*

RUN grep '^rsession-ld-library-path=/opt/conda/lib' /etc/rstudio/rserver.conf || echo 'rsession-ld-library-path=/opt/conda/lib' >> /etc/rstudio/rserver.conf

USER $NB_UID

# Install R Geospatial packages
RUN mamba install --quiet --yes \
    'r-raster' \
    'r-rastervis' \
    'r-remotes' \
    'r-reshape' \
    'r-rgdal' \
    'r-sf' \
    && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_UID

WORKDIR $HOME
