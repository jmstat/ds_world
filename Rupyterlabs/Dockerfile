# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG OWNER=jupyter
ARG BASE_CONTAINER=$OWNER/r-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"
LABEL editor="Jimmy Lim"

# ### jupyter-rssesion-proxy
# RUN python3 -m pip install jupyter-rsession-proxy
# RUN cd /tmp/ && \
#     git clone --depth 1 https://github.com/jupyterhub/jupyter-server-proxy && \
#     cd jupyter-server-proxy/jupyterlab-server-proxy && \
#     npm install && npm run build && jupyter labextension link . && \
#     npm run build && jupyter lab build

### install rstudio-server
USER root
RUN apt-get update --yes -qq

# # rstudio dependency lib
# RUN apt-get update --yes -qq && \
#     wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0.0_1.0.2n-1ubuntu5.7_amd64.deb && \
#     dpkg -i libssl1.0.0_1.0.2n-1ubuntu5.7_amd64.deb

# # install rstudio-server
# # RUN curl --silent -L --fail https://download2.rstudio.org/server/bionic/amd64/rstudio-server-2021.09.0-351-amd64.deb > /tmp/rstudio.deb && \
# RUN wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-2021.09.0-351-amd64.deb && \
#     mv rstudio-server-2021.09.0-351-amd64.deb /tmp/rstudio.deb && \
#     apt-get install -y libc6 && \
#     # echo '24cd11f0405d8372b4168fc9956e0386 /tmp/rstudio.deb' | md5sum -c - && \
#     apt-get install -y /tmp/rstudio.deb && \
#     rm /tmp/rstudio.deb
# ENV PATH=$PATH:/usr/lib/rstudio-server/bin
# ENV RSESSION_PROXY_RSTUDIO_1_4=1

## install pyodbc
RUN apt-get install -y --no-install-recommends build-essential gcc && \
    apt-get install -y python3.9-dev

# for MS ODBC driver, psycopg2 (postgres)
RUN wget https://packages.microsoft.com/ubuntu/20.04/prod/pool/main/m/msodbcsql17/msodbcsql17_17.8.1.1-1_amd64.deb && \
    ACCEPT_EULA=Y dpkg -i msodbcsql17_17.8.1.1-1_amd64.deb && \
    pip install pyodbc
RUN conda install --quiet --yes psycopg2

# ffmpeg for matplotlib anim & dvipng+cm-super for latex labels
RUN apt-get install --yes --no-install-recommends ffmpeg dvipng cm-super && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

### pyodbc conda - after
# Install Python 3 packages
RUN conda install --quiet --yes \
    'altair=4.1.*' \
    'beautifulsoup4=4.9.*' \
    'bokeh=2.3.*' \
    'bottleneck=1.3.*' \
    'cloudpickle=1.6.*' \
    'conda-forge::blas=*=openblas' \
    'cython=0.29.*' \
    'dask=2021.6.*' \
    'dill=0.3.*' \
    'h5py=3.3.*' \
    'ipympl=0.7.*'\
    'ipywidgets=7.6.*' \
    'matplotlib-base=3.4.*' \
    'numba=0.53.*' \
    'numexpr=2.7.*' \
    'pandas=1.3.*' \
    'patsy=0.5.*' \
    'protobuf=3.17.*' \
    'pytables=3.6.*' \
    'scikit-image=0.18.*' \
    'scikit-learn=0.24.*' \
    'scipy=1.7.*' \
    'seaborn=0.11.*' \
    'sqlalchemy=1.4.*' \
    'statsmodels=0.12.*' \
    'sympy=1.8.*' \
    'widgetsnbextension=3.5.*'\
    'xlrd=2.0.*' && \
    conda clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install facets which does not have a pip or conda package at the moment
WORKDIR /tmp
RUN git clone https://github.com/PAIR-code/facets.git && \
    jupyter nbextension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"

RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"

WORKDIR "${HOME}"