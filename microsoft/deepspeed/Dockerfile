FROM pytorch/pytorch:2.3.0-cuda11.8-cudnn8-devel

# I haven't spend too much time optimizing these packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yq --no-install-recommends \
    software-properties-common \
    build-essential \
    autotools-dev \
    pdsh \
    cmake \
    g++ \
    gcc \
    llvm llvm-dev \
    git \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# Install oneCCL
# DeepSpeed with DS_BUILD_OPS=1 requires it
# The lates version (released on 2024-3-37) is 2021.12. Funny
ENV ONECCL_VERSION=2021.12
RUN git clone --depth 1 --branch ${ONECCL_VERSION} https://github.com/oneapi-src/oneCCL.git /tmp/oneccl \
  && cd /tmp/oneccl \
  && mkdir build \
  && cd build \
  && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local \
  && make -j install

# Install DeepSpeed
ENV DS_BUILD_OPS=1
RUN pip install deepspeed
