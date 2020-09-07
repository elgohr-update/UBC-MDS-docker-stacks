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
    nodejs \
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
  r-janitor \
  r-reticulate \
  r-testthat=2.3.* \
  r-rpostgres=1.2.*

# Install R packages from CRAN
RUN Rscript -e "install.packages('palmerpenguins', repos='http://cran.us.r-project.org')" \
    && Rscript -e "install.packages('repurrrsive', repos='http://cran.us.r-project.org')"

# Install R packages from GitHub
RUN Rscript -e "devtools::install_github('ttimbers/canlang@0.0.1')"

USER root

# Install jupyter extensions (nbgitpuller, git, jupytext)
RUN pip install git+https://github.com/data-8/nbgitpuller \
    && jupyter serverextension enable --sys-prefix nbgitpuller \
    && jupyter labextension install @jupyterlab/git \
    && pip install jupyterlab-git \
    && jupyter serverextension enable --py jupyterlab_git \
    && pip install jupytext --upgrade \
    && jupyter labextension install @techrah/text-shortcuts \
    && jupyter labextension install @jupyter-widgets/jupyterlab-manager \
    && jupyter lab build 
RUN useradd -m -s /bin/bash -N -u 9999 jupyter

# install nano text editor & man pages & customize terminal
RUN apt update \
    && apt install nano \
    && apt install -y man-db manpages-posix \
    && bind "set completion-ignore-case on" \
    && bind "set show-all-if-ambiguous on" \
    && bind 'TAB':menu-complete \
    && bind "set menu-complete-display-prefix on" \
    && bind '"\e[A":history-search-backward' \
    && bind '"\e[B":history-search-forward' \
    && shopt -s histappend
ENV PROMPT_COMMAND 'history -a; echo'
ENV HISTSIZE 50000
ENV HISTFILESIZE 50000

# set nano as text editor
ENV VISUAL nano
ENV EDITOR "$VISUAL"

# install canvautils
RUN pip install canvasutils

# Configure jupyter user
USER jupyter
ENV NB_USER=jupyter \
    NB_UID=9999
ENV HOME=/home/$NB_USER
WORKDIR $HOME
