# OSU-Carpentry Jupyter Base
# (i.e. jupyter/datascience-notebook with RStuio)
# This image should have all software needed to support the following lessons:
#    Software Carpentry (all core lessons)
#    Data Carpentry Ecology (R and Python)
#    Data Carpentry Social Science (R)

ARG BASE_CONTAINER=quay.io/jupyter/datascience-notebook:8890fc557a2c
FROM $BASE_CONTAINER

LABEL maintainer="Evan Linde <elinde@okstate.edu>" \
    description="OSU-Carpentry Jupyter Base"

USER root

# Dependencies for RStudio
RUN DEBIAN_FRONTENT=noninteractive \
    apt-get update && \
    apt-get -y dist-upgrade && \
    apt-get install -y --no-install-recommends \
    gdebi-core \
    psmisc \
    libapparmor1 \
    libpq5 \
    libssl-dev \
    lsb-release \
    libclang-dev \
    zip unzip \
    less \
    lmodern \
    && \
    apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/*

# Install RStudio
ENV RSESSION_PROXY_RSTUDIO_1_4=yes
ENV RSTUDIO_VERSION=2024.04.2-764
ENV RSTUDIO_SHA256=d8ffb8e5c2dadb1c74df037c291a5585e595d978b3aec48a4a94a970e04a947d
ENV RSTUDIO_DEB=rstudio-server-${RSTUDIO_VERSION}-amd64.deb
ENV RSTUDIO_URL=https://download2.rstudio.org/server/jammy/amd64/${RSTUDIO_DEB}
WORKDIR /tmp
RUN wget --quiet ${RSTUDIO_URL} && \
    echo "${RSTUDIO_SHA256} *${RSTUDIO_DEB}" | sha256sum -c - && \
    gdebi --quiet --non-interactive ${RSTUDIO_DEB} && \
    rm -f ${RSTUDIO_DEB} && \
    unset RSTUDIO_VERSION RSTUDIO_SHA256 RSTUDIO_DEB RSTUDIO_URL
RUN grep '^session-timeout-minutes=0' /etc/rstudio/rsession.conf || echo 'session-timeout-minutes=0' >> /etc/rstudio/rsession.conf

USER $NB_UID

# Start RStudio from within Jupyter
RUN pip install --quiet jupyter-rsession-proxy jupyter-server-proxy jupyterlab_latex && \
    R -e "install.packages('markdown', repos='https://cran.rstudio.com')" && \
    rm -rf "/home/${NB_USER}/.cache" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_UID

WORKDIR $HOME
