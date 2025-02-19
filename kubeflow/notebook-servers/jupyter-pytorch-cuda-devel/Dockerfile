ARG BASE_IMG=<base>
FROM $BASE_IMG

ARG CUDA_VERSION=12.1.1

# We still need to specify the channel. for all the dependencies.
# See <https://stackoverflow.com/a/78843983/6564721>
RUN conda install nvidia/label/cuda-${CUDA_VERSION}::cuda-toolkit -c nvidia/label/cuda-${CUDA_VERSION} -y

# CUDA 12.1.1 supports gcc 12.2
# See <https://docs.nvidia.com/cuda/archive/12.1.1/cuda-installation-guide-linux/index.html#system-requirements>
ARG GCC_VERSION=12.2
RUN conda install gcc=${GCC_VERSION} gxx=${GCC_VERSION} -y
