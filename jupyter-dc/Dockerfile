ARG BASE_CONTAINER=osucarpentry/jupyter-base:latest
FROM $BASE_CONTAINER

LABEL maintainer="Evan Linde <elinde@okstate.edu>"

USER root

# Dependencies for OpenRefine
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    default-jre \
    && \
    rm -rf /var/lib/apt/lists/*

# Install OpenRefine
WORKDIR /opt
ENV OPENREFINE_VERSION=3.6.1
ENV OPENREFINE_URL="https://oss.sonatype.org/service/local/artifact/maven/content?r=releases&g=org.openrefine&a=openrefine&v=${OPENREFINE_VERSION}&c=linux&p=tar.gz"
RUN wget --quiet -O openrefine.tar.gz ${OPENREFINE_URL} && \
    tar -zxf openrefine.tar.gz && \
    if [[ -f openrefine ]]; then unlink openrefine; fi && \
    ln -s openrefine-${OPENREFINE_VERSION} openrefine && \
    rm -f openrefine-linux-${OPENREFINE_VERSION}.tar.gz && \
    unset OPENREFINE_VERSION OPENREFINE_URL

USER $NB_UID

# Start OpenRefine from within Jupyter
RUN pip install --quiet git+https://github.com/evanlinde/jupyter-openrefine-proxy
RUN jupyter labextension install @jupyterlab/server-proxy && \
    jupyter lab clean -y && \
    npm cache clean --force && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    rm -rf "/home/${NB_USER}/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_UID

WORKDIR $HOME
