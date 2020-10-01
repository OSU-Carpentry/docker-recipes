# OSU-Carpentry Jupyter Base
# (i.e. jupyter/datascience-notebook with RStuio)
# This image should have all software needed to support the following lessons:
#    Software Carpentry (all core lessons)
#    Data Carpentry Ecology (R and Python)
#    Data Carpentry Social Science (R)

ARG BASE_CONTAINER=jupyter/datascience-notebook:6d42503c684f
FROM $BASE_CONTAINER

LABEL maintainer="Evan Linde <elinde@okstate.edui>" \
    description="OSU-Carpentry Jupyter Base"

USER root

# Dependencies for RStudio
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    gdebi-core \
    psmisc \
    libapparmor1 \
    lsb-release \
    libclang-dev \
    && \
    rm -rf /var/lib/apt/lists/*

# Install RStudio
#ENV RSTUDIO_VERSION=1.3.1093 # This one doesn't work either
#ENV RSTUDIO_SHA256=47022541152d0b586c810ab23c36ee2fc0d7e0d1b69c554dfcd7fe00d0fb571b
#ENV RSTUDIO_VERSION=1.3.1073 # There's currently a bug with this version
#ENV RSTUDIO_SHA256=bd9a6b0f40886e632de01070378e7fe942ca15fff1d185e42ddab1801e9e2a75
ENV RSTUDIO_VERSION=1.2.5001
ENV RSTUDIO_DEB=rstudio-server-${RSTUDIO_VERSION}-amd64.deb
ENV RSTUDIO_URL=https://download2.rstudio.org/server/bionic/amd64/${RSTUDIO_DEB}
WORKDIR /tmp
RUN wget --quiet ${RSTUDIO_URL} && \
    #echo "${RSTUDIO_SHA256} *${RSTUDIO_DEB}" | sha256sum -c - && \
    gdebi --quiet --non-interactive ${RSTUDIO_DEB} && \
    rm -f ${RSTUDIO_DEB}

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
