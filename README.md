# ZJUICI Container Images

This repository contains common container images used by ZJUICI.

## Kubeflow Notebook Server Images

We have created custom Jupyter images that extend the [official example notebook servers](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers) to be used as Kubeflow notebook servers.

In most senarios, the official example images should meet your needs. If necessary, you can easily upgrade Python packages using `mamba`. Our customizations to these images address the following points:

- Some base libraries, such as `jupyterlab`, are pinned to a specific version and are not regularly updated. We keep them in sync with the latest version.
- The `-pytorch` example images only include Pytorch and its own cuda runtime, but not the CUDA compiler. Certain deep learning libraries such as `DeepSpeed` require the CUDA compiler to be present when installing.
- Some packages, such as `DeepSpeed` and `vllm`, depend on specific CUDA versions. Unaware users may install versions that require a different CUDA version from the system-provided one, which can potentially break the CUDA environment.

You can explore all our customized notebook server images [here](https://github.com/orgs/ZJUICI/packages?visibility=public&tab=packages&q=kubeflow-notebooks).
