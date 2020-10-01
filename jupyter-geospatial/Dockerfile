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

USER $NB_UID

# Install R Geospatial packages
RUN conda install --quiet --yes \
    'r-raster' \
    'r-rastervis' \
    'r-remotes' \
    'r-sf' \
    && \
    conda clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install rgdal
RUN R -e 'install.packages("rgdal", repos="https://cran.rstudio.com")'

USER $NB_UID

WORKDIR $HOME
