# OSU-Carpentry Jupyter Genomics
# This image should have all software needed to support the Data Carpentry Genomics lessons.

ARG BASE_CONTAINER=osucarpentry/jupyter-base:latest
FROM $BASE_CONTAINER

LABEL maintainer="Evan Linde <elinde@okstate.edu>" \
    description="OSU-Carpentry Jupyter Genomics"

USER root

# Geospatial dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fastqc \
    trimmomatic \
    bwa \
    samtools \
    bcftools \
    igv \
    && \
    rm -rf /var/lib/apt/lists/*

USER $NB_UID

WORKDIR $HOME
