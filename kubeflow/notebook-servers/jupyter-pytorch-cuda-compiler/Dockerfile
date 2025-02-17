ARG BASE_IMG=<base>
FROM $BASE_IMG

ARG CUDA_VERSION=12.1.1

# We still need to specify the channel. for all the dependencies.
# See <https://stackoverflow.com/a/78843983/6564721>
RUN conda install nvidia/label/cuda-${CUDA_VERSION}::cuda-compiler -c nvidia/label/cuda-${CUDA_VERSION} -y
