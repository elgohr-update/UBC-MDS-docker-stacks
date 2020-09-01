# Copyright (c) UBC-DSCI Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=jupyter/r-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Tiffany Timbers <tiffany.timbers@stat.ubc.ca>"

# Install Python packages on conda-forge
RUN conda install --quiet --yes -c conda-forge \
    altair=4.* \
    black=19.* \
    flake8=3.* \
    pandas-profiling=1.* \
    pytest=6.* \
    seaborn=0.10.*

# Install R packages on conda-forge
RUN conda install --quiet --yes -c conda-forge \
  r-cowplot=1.* \
  r-gapminder \
  r-ggally=1.5.* \
  r-gridextra=2.3.* \
  r-infer=0.5.* \
  r-testthat=2.3.* \
  r-rpostgres=1.2.*

# Install R packages from CRAN
RUN Rscript -e "install.packages('palmerpenguins', repos='http://cran.us.r-project.org')"

# Install R packages from GitHub
RUN Rscript -e "devtools::install_github('UBC-MDS/canlang@0.0.1')"

# Install jupyter extensions (nbgitpuller, git, jupytext)
USER root

RUN pip install git+https://github.com/data-8/nbgitpuller \
    && jupyter serverextension enable --sys-prefix nbgitpuller \
    && jupyter labextension install @jupyterlab/git \
    && pip install jupyterlab-git \
    && jupyter serverextension enable --py jupyterlab_git \
    && pip install jupytext --upgrade \
    && jupyter labextension install @ryantam626/jupyterlab_code_formatter \
    && pip install jupyterlab_code_formatter \
    && jupyter serverextension enable --py jupyterlab_code_formatter \
    && jupyter labextension install @techrah/text-shortcuts \
    && jupyter lab build 
RUN useradd -m -s /bin/bash -N -u 9999 jupyter

USER jupyter

# Configure jupyter user
ENV NB_USER=jupyter \
    NB_UID=9999
ENV HOME=/home/$NB_USER
WORKDIR $HOME
