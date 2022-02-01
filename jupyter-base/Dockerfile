# OSU-Carpentry Jupyter Base
# (i.e. jupyter/datascience-notebook with RStuio)
# This image should have all software needed to support the following lessons:
#    Software Carpentry (all core lessons)
#    Data Carpentry Ecology (R and Python)
#    Data Carpentry Social Science (R)

ARG BASE_CONTAINER=jupyter/datascience-notebook:cf5a7ab55638
FROM $BASE_CONTAINER

LABEL maintainer="Evan Linde <elinde@okstate.edui>" \
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
    lsb-release \
    libclang-dev \
    zip unzip \
    less \
    && \
    apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/*

# Install RStudio
ENV RSESSION_PROXY_RSTUDIO_1_4=yes
ENV RSTUDIO_VERSION=2021.09.2-382
ENV RSTUDIO_SHA256=4941e198d855a3d0531b6fe969db3373ee24d0bed815f276f04cc3b349a9d1bb
ENV RSTUDIO_DEB=rstudio-server-${RSTUDIO_VERSION}-amd64.deb
ENV RSTUDIO_URL=https://download2.rstudio.org/server/bionic/amd64/${RSTUDIO_DEB}
WORKDIR /tmp
RUN wget --quiet ${RSTUDIO_URL} && \
    echo "${RSTUDIO_SHA256} *${RSTUDIO_DEB}" | sha256sum -c - && \
    gdebi --quiet --non-interactive ${RSTUDIO_DEB} && \
    rm -f ${RSTUDIO_DEB}
RUN grep '^session-timeout-minutes=0' /etc/rstudio/rsession.conf || echo 'session-timeout-minutes=0' >> /etc/rstudio/rsession.conf

USER $NB_UID

# Start RStudio from within Jupyter
RUN pip install --quiet jupyter-rsession-proxy
RUN jupyter labextension install @jupyterlab/server-proxy && \
    jupyter lab clean -y && \
    npm cache clean --force && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    rm -rf "/home/${NB_USER}/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_UID

WORKDIR $HOME
