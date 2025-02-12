# ZJUICI Container Images

This repository contains common images used by ZJUICI, with some of them being kubeflow jupyter images.

## Jupyter Images

> See <https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers> for more information.

This section contains the Dockerfiles for the Jupyter images that can be used in kubeflow.

There are currently 2 custom images:

- jupyter lab
  - JupyterLab image with [DeepSpeed](https://github.com/microsoft/DeepSpeed)
- codeserver
  - code-server image with [DeepSpeed](https://github.com/microsoft/DeepSpeed)

### Core Dependencies

image | tag | DeepSpeed | PyTorch | cuda | cudnn
---|---|---|---|---|---
zjuici/jupyter | deepspeed-0.16.3 | v0.16.3 | 2.5.1 | 12.4 | cudnn9
zjuici/codeserver | deepspeed-0.16.3 | v0.16.3 | 2.5.1 | 12.4 | cudnn9
