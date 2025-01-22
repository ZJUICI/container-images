# Installing deepspeed requires $CUDA_HOME and probably nvcc, which is not present in -runtime images
# Maybe I can optimize this by multi-stage build, but I don't have bandwidth to do it now.
FROM pytorch/pytorch:2.5.1-cuda12.4-cudnn9-devel

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
# The lates version (released on 2024-11-06) is 2021.14. Funny
ENV ONECCL_VERSION=2021.14
RUN git clone --depth 1 --branch ${ONECCL_VERSION} https://github.com/uxlfoundation/oneCCL.git /tmp/oneccl \
  && cd /tmp/oneccl \
  && mkdir build \
  && cd build \
  && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local \
  && make -j install

# Install DeepSpeed
ENV DS_BUILD_OPS=1
RUN pip install deepspeed==0.16.3
