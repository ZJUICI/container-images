FROM kubeflownotebookswg/jupyter-scipy:v1.9.2

ARG JUPYTERLAB_VERSION=4.3.5
ARG JUPYTER_VERSION=7.3.2

RUN sed -i "s/jupyterlab ==[0-9.]*$/jupyterlab ==$JUPYTERLAB_VERSION/" ${CONDA_DIR}/conda-meta/pinned \
  && sed -i "s/notebook ==[0-9.]*$/notebook ==$JUPYTER_VERSION/" ${CONDA_DIR}/conda-meta/pinned \
  # With modified pins we have to use conda instead of mamba to install these packages first.
  && conda install -y -q \
    jupyterlab==${JUPYTERLAB_VERSION} \
    notebook==${JUPYTER_VERSION} \
    ipywidgets \
    jupyter-resource-usage \
    jupyterlab-git \
  && conda clean -a -f -y

# We upgrade some heavy dependencies here.
# It's easy for users to update light weight deps on their own.
RUN mamba upgrade -y -q \
    scipy \
    scikit-learn \
    scikit-image \
  && mamba clean -a -f -y

# Backup the home dir again.
# Not sure if it's needed but it won't take too long nor too much image size.
RUN cp -p -r -T ${HOME} ${HOME_TMP} \
  && chmod -R g=u ${HOME_TMP}
