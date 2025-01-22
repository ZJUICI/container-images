# ZJUICI Container Images

This repository contains common images used by ZJUICI, with some of them being kubeflow jupyter images, and some of them serving as mirrors for gcr.io, ghcr.io, or registry.k8s.io.

## Jupyter Images

> See <https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers> for more information.

This section contains the Dockerfiles for the Jupyter images that can be used in kubeflow.

There are currently 8 different images:

- jupyter lab
  - basic [JupyterLab](https://github.com/jupyterlab/jupyterlab) image
  - JupyterLab image with [PyTorch](https://github.com/pytorch/pytorch)
  - JupyterLab image with [DeepSpeed](https://github.com/microsoft/DeepSpeed)
  - JupyterLab image with [text-generation-inference](https://github.com/huggingface/text-generation-inference)
  - JupyterLab image with [text-embedding-inference](https://github.com/huggingface/text-embeddings-inference)
- codeserver
  - basic [code-server](https://github.com/coder/code-server) image
  - code-server image with [PyTorch](https://github.com/pytorch/pytorch)
  - code-server image with [DeepSpeed](https://github.com/microsoft/DeepSpeed)
  - code-server image with [text-generation-inference](https://github.com/huggingface/text-generation-inference)
  - code-server image with [text-embedding-inference](https://github.com/huggingface/text-embeddings-inference)

The basic idea is that we provide images with some complicated dependencies pre-installed, so that users can get started with their work without having to worry about the installation process. These complicated dependencies includes:

- PyTorch: not too difficult to install, but it's a large package and it takes a long time.
- DeepSpeed: requires compile from source to enable all features, which has complicated dependency matrix.
- text-generation-inference: requires to build a lot of kernels, while provides an ease-to-use Docker image. We leverage its Docker image.
- text-embedding-inference: requires some effort to install locally, while provides some ease-to-use Docker images. Because it's a development setup, we pick the CPU image as the base image.

> When mounting a volume as huggingface's cache directory, it's important to be aware that both text-generation-inference and text-embedding-inference Dockerfiles set  `HUGGINGFACE_HUB_CACHE` to `/data`.
>
> See [text-generation-inference](https://github.com/huggingface/text-generation-inference/blob/c2d4a3b5c7bb6a8367c00f7c797bf87f4b2fcef9/Dockerfile#L170) and [text-embedding-inference](https://github.com/huggingface/text-embeddings-inference/blob/282812743444c33f9e5f4f3681dbbe2472fd651e/Dockerfile#L72)

### Core Dependencies

image | tag | PyTorch | cuda | cudnn
---|---|---|---|---
zjuici/jupyter-pytorch | 0.1.0 | 2.3.0 | 12.1 | cudnn8
zjuici/codeserver-pytorch | 0.1.0 | 2.3.0 | 12.1 | cudnn8

image | tag | DeepSpeed | PyTorch | cuda | cudnn
---|---|---|---|---|---
zjuici/jupyter-deepspeed | 0.1.0 | v0.14.2 | 2.3.0 | 11.8 | cudnn8
zjuici/codeserver-deepspeed | 0.1.0 | v0.14.2 | 2.3.0 | 11.8 | cudnn8

image | tag | TGI | PyTorch | cuda | cudnn
---|---|---|---|---|---
zjuici/jupyter | 0.8.0-tgi | v1.4.4 | 2.1.1 | 12.1.0 | cudnn8
zjuici/codeserver | 0.8.1-tgi | v1.4.4 | 2.1.1 | 12.1.0 | cudnn8

image | tag | TEI
---|---|---
zjuici/jupyter | 0.2.0-tei-cpu | v1.2.0
zjuici/codeserver | 0.2.1-tei-cpu | v1.2.0
