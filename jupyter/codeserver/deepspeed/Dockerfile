FROM pytorch/pytorch:1.13.1-cuda11.6-cudnn8-devel

# common environemnt variables
ENV NB_USER jovyan
ENV NB_UID 1000
ENV NB_PREFIX /
ENV HOME /home/$NB_USER
ENV SHELL /bin/bash

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yq --no-install-recommends \
    apt-transport-https \
    bzip2  \
    ca-certificates \
    curl \
    git \
    locales \
    lsb-release \
    unzip \
    vim \
    wget \
    xz-utils \
    zip \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --upgrade --no-cache-dir pip 

# install - s6 overlay
ARG S6_OVERLAY_VERSION=v3.1.6.2
ARG S6_OVERLAY_ARCH="x86_64"
RUN curl -LO "https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz" \
  && curl -LO "https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz.sha256" \
  && sha256sum -c s6-overlay-noarch.tar.xz.sha256 \
  && tar -C / -Jxpf s6-overlay-noarch.tar.xz \
  && rm -f s6-overlay-noarch.tar.xz \
  && rm -f s6-overlay-noarch.tar.xz.sha256 \
  && curl -LO "https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-${S6_OVERLAY_ARCH}.tar.xz" \
  && curl -LO "https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-${S6_OVERLAY_ARCH}.tar.xz.sha256" \
  && sha256sum -c s6-overlay-${S6_OVERLAY_ARCH}.tar.xz.sha256 \
  && tar -C / -Jxpf s6-overlay-${S6_OVERLAY_ARCH}.tar.xz \
  && rm -f s6-overlay-${S6_OVERLAY_ARCH}.tar.xz \
  && rm -f s6-overlay-${S6_OVERLAY_ARCH}.tar.xz.sha256

# install - kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
  && curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256" \
  && echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check \
  && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# install - code-server
# pytorch 1.13 use ubuntu 18.04 as base image, which ships node v
# the latest code-server supports node v is 4.16.1
ARG CODESERVER_VERSION=4.16.1
RUN curl -LO "https://github.com/cdr/code-server/releases/download/v${CODESERVER_VERSION}/code-server_${CODESERVER_VERSION}_amd64.deb" \
  && dpkg -i code-server_${CODESERVER_VERSION}_amd64.deb \
  && rm -f code-server_${CODESERVER_VERSION}_amd64.deb

RUN code-server --install-extension ms-python.python

# install triton 1.0.0
# should be as easy as `pip install triton==1.0.0`, but my pip can only find triton==2.0.0
# see more @ <https://github.com/openai/triton/>
ENV TRITON_VERSION=v1.0
RUN apt-get update && apt-get install -yq --no-install-recommends \
    libtinfo-dev \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
RUN python -m pip install --no-cache-dir ninja cmake wheel
RUN git clone https://github.com/openai/triton.git /tmp/triton \
  && cd /tmp/triton \
  && git checkout ${TRITON_VERSION} \
  && pip install -e python

# install deepspeed
# pytorch 1.13 image ships nvcc in /usr/local/cuda/bin, which is not the correct nvcc
# see <https://github.com/microsoft/DeepSpeed/issues/2684#issuecomment-1379666816>
ENV PATH=/usr/local/cuda/bin:$PATH
# TODO: migrate to newer versions
# DeepSpeed >= v0.11.0 complains no async_io, although I have installed it.
# DeepSpeed >- v0.10.0 complains about something else.
ENV DEEPSPEED_VERSION=v0.9.5
ENV DS_BUILD_OPS=1
RUN apt-get update && apt-get install -yq --no-install-recommends \
    libaio1 \
    libaio-dev \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/microsoft/DeepSpeed.git /tmp/DeepSpeed \
  && cd /tmp/DeepSpeed \
  && git checkout ${DEEPSPEED_VERSION} \
  && ./install.sh --allow_sudo \
  && cd .. && rm -rf /tmp/DeepSpeed

# create user and set required ownership
RUN useradd -M -s /bin/bash -N -u ${NB_UID} ${NB_USER}
RUN mkdir -p ${HOME} \
  && chown -R ${NB_USER}:users ${HOME} \
  && chown -R ${NB_USER}:users /usr/local/bin \
  && chown -R ${NB_USER}:users /etc/s6-overlay

# s6 - copy scripts
COPY --chown=${NB_USER}:users s6/ /etc/
RUN chmod a+x /etc/services.d/code-server/*

USER $NB_UID
# path for pip install --user
ENV PATH=${HOME}/.local/bin/:${PATH}

EXPOSE 8888

ENTRYPOINT ["/init"]
