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
zjuici/jupyter | 0.6.1-pytorch | 2.0.1 | 11.7 | cudnn8
zjuici/codeserver | 0.6.1-pytorch | 2.0.1 | 11.7 | cudnn8

image | tag | DeepSpeed | PyTorch | cuda | cudnn
---|---|---|---|---|---
zjuici/jupyter | 0.6.1-deepspeed | v0.9.5 | 1.13.1 | 11.6 | cudnn8
zjuici/codeserver | 0.5.1-deepspeed | v0.9.5 | 1.13.1 | 11.6 | cudnn8

image | tag | TGI | PyTorch | cuda | cudnn
---|---|---|---|---|---
zjuici/jupyter | 0.7.4-tgi | v1.4.0 | 2.1.1 | 12.1.0 | cudnn8
zjuici/codeserver | 0.7.4-tgi | v1.4.0 | 2.1.1 | 12.1.0 | cudnn8

image | tag | TEI
---|---|---|---
zjuici/jupyter | 0.1.0-tei-cpu | v0.6.0
zjuici/codeserver | 0.1.0-tei-cpu | v0.6.0

## Mirrored Images

There exists images that are difficult to pull in some countries, such as images on ghcr or gcr. This project is to mirror those images to docker hub that is more accessible in such countries.

### cert-manager

Source Image | Mirrored Image | Latest Tag | Original Repo
--- | --- | --- | ---
quay.io/jetstack/cert-manager-cainjector | zjuici/mirror.jetstack.cert-manager-cainjector | v1.13.2 | <https://github.com/cert-manager/cert-manager>
quay.io/jetstack/cert-manager-controller | zjuici/mirror.jetstack.cert-manager-controller | v1.13.2 | <https://github.com/cert-manager/cert-manager>
quay.io/jetstack/cert-manager-webhook | zjuici/mirror.jetstack.cert-manager-webhook | v1.13.2 | <https://github.com/cert-manager/cert-manager>

### Huggingface

Source Image | Mirrored Image | Latest Tag | Original Repo
--- | --- | --- | ---
ghcr.io/huggingface/text-generation-inference | zjuici/mirror.huggingface.text-generation-inference | 1.4.0 | <https://github.com/huggingface/text-generation-inference/>
ghcr.io/huggingface/text-embeddings-inference | zjuici/mirror.huggingface.text-embeddings-inference | 0.6.0 | <https://github.com/huggingface/text-embeddings-inference/>

### Knative

Source Image | Mirrored Image | Latest Tag | Original Repo
--- | --- | --- | ---
gcr.io/knative-releases/knative.dev/serving/cmd/activator | zjuici/mirror.knative-serving.serving-core.activator | v1.11.0 | <https://github.com/knative/serving/>
gcr.io/knative-releases/knative.dev/serving/cmd/autoscaler | zjuici/mirror.knative-serving.serving-core.autoscaler | v1.11.0 | <https://github.com/knative/serving/>
gcr.io/knative-releases/knative.dev/serving/cmd/controller | zjuici/mirror.knative-serving.serving-core.controller | v1.11.0 | <https://github.com/knative/serving/>
gcr.io/knative-releases/knative.dev/serving/cmd/webhook | zjuici/mirror.knative-serving.serving-core.webhook | v1.11.0 | <https://github.com/knative/serving/>
gcr.io/knative-releases/knative.dev/serving/cmd/queue | zjuici/mirror.knative-serving.serving-core.queue | v1.11.0 | <https://github.com/knative/serving/>
gcr.io/knative-releases/knative.dev/net-istio/cmd/controller | zjuici/mirror.knative-serving.net-istio.controller | v1.11.0 | <https://github.com/knative-extensions/net-istio/>
gcr.io/knative-releases/knative.dev/net-istio/cmd/webhook | zjuici/mirror.knative-serving.net-istio.webhook | v1.11.0 | <https://github.com/knative-extensions/net-istio/>

### Kubernetes

Source Image | Mirrored Image | Latest Tag | Original Repo
--- | --- | --- | ---
gcr.io/kubebuilder/kube-rbac-proxy | zjuici/mirror.kubebuilder.kube-rbac-proxy | v0.14.2 | <https://github.com/brancz/kube-rbac-proxy>
registry.k8s.io/kube-state-metrics/kube-state-metrics | zjuici/mirror.kube-state-metrics.kube-state-metrics | v2.9.2 | <https://github.com/kubernetes/kube-state-metrics>
registry.k8s.io/sig-storage/nfs-subdir-external-provisioner | zjuici/mirror.kubernetes-sigs.nfs-subdir-external-provisioner | v4.0.2 | <https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner>
registry.k8s.io/pause | zjuici/mirror.pause | 3.8 | <https://github.com/kubernetes/kubernetes/tree/master/build/pause>
registry.k8s.io/prometheus-adapter/prometheus-adapter | zjuici/mirror.prometheus-adapter.prometheus-adapter | v0.11.1 | <https://github.com/kubernetes-sigs/prometheus-adapter>

### prometheus-operator

Source Image | Mirrored Image | Latest Tag | Original Repo
--- | --- | --- | ---
quay.io/prometheus-operator/prometheus-operator | zjuici/mirror.prometheus-operator.prometheus-operator | v0.67.1 | <https://github.com/prometheus-operator/prometheus-operator>
quay.io/prometheus-operator/prometheus-config-reloader | zjuici/mirror.prometheus-operator.prometheus-config-reloader | v0.67.1 | <https://github.com/prometheus-operator/prometheus-operator>

### OAuth2 Proxy

Source Image | Mirrored Image | Latest Tag | Original Repo
--- | --- | --- | ---
quay.io/oauth2-proxy/oauth2-proxy | zjuici/mirror.oauth2-proxy.oauth2-proxy | v7.5.1 | <https://github.com/oauth2-proxy/oauth2-proxy>
