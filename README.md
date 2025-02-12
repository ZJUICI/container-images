# ZJUICI Container Images

This repository contains common images used by ZJUICI.

## Kubeflow Notebook Server Images

This section contains the Jupyter images that can be utilized as Kubeflow notebook servers. Typically, the images available at <https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers> will meet your requirements. If needed, you can easily upgrade Python packages using `mamba`.

We have extended these images primarily for the following reasons:

- The version of `jupyterlab` is pinned in the example notebook images. We have upgraded it to enhance the user experience.
- Some packages, such as `scikit-learn` and `deepspeed`, take a considerable amount of time to install. By pre-installing these in our custom images, we reduce the setup time for users.

### Core Dependencies

image | tag | DeepSpeed | PyTorch | cuda | cudnn
---|---|---|---|---|---
zjuici/jupyter | deepspeed-0.16.3 | v0.16.3 | 2.5.1 | 12.4 | cudnn9
zjuici/codeserver | deepspeed-0.16.3 | v0.16.3 | 2.5.1 | 12.4 | cudnn9
