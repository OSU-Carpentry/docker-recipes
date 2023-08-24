# OSU-Carpentry Jupyter Base
# (i.e. jupyter/datascience-notebook with RStuio)
# This image should have all software needed to support the following lessons:
#    Software Carpentry (all core lessons)
#    Data Carpentry Ecology (R and Python)
#    Data Carpentry Social Science (R)

ARG BASE_CONTAINER=jupyter/datascience-notebook:d400e383cc1d
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
ENV RSTUDIO_VERSION=2023.06.1-524
ENV RSTUDIO_SHA256=208897f00b580b45c37dbb787dc482ff6d774b32c230f49578691b1a11f40c37
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
    rm -rf "/home/${NB_USER}/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
RUN R -e "install.packages('markdown', repos='https://cran.rstudio.com')"

USER $NB_UID

WORKDIR $HOME
