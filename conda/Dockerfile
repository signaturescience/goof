FROM ubuntu:latest
ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"
# install required packages
RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y curl git python3 python3-pip

# # install python packages
# RUN pip3 install -U matplotlib

# install miniconda3
RUN curl -LO https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN chmod +x ./Miniconda3-latest-Linux-x86_64.sh
RUN ./Miniconda3-latest-Linux-x86_64.sh -b

COPY . /src/conda

RUN conda update -n base conda
# RUN conda config --set channel_priority strict
RUN conda env update --name goof --file /src/conda/environment.yml --prune

ENTRYPOINT [ "conda", "activate", "goof" ]
