# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.134.1/containers/alpine/.devcontainer/base.Dockerfile
# Taken from: https://github.com/kayceesrk/cs3100_m20/blob/gh-pages/_docker/dockerfile
ARG OCAML_VERSION="4.11"
FROM ocurrent/opam:debian-10-ocaml-${OCAML_VERSION}

RUN sudo apt update --fix-missing
RUN sudo apt install -y libgmp-dev libzmq3-dev m4 perl pkg-config zlib1g-dev python3 \
  python3-pip --fix-missing

RUN pip3 install jupyter-book "jupytext==1.7.1" pandas matplotlib "sphinxcontrib-bibtex<2.0.0"
RUN pip3 install --no-cache-dir --no-warn-script-location jupyter-book "jupytext==1.7.1" pandas matplotlib "sphinxcontrib-bibtex<2.0.0" numpy jupyter requests dash


# OCaml + Jupyter
RUN opam remote add upstream https://github.com/ocaml/opam-repository.git && \
    opam update && opam upgrade

# Python libs
RUN pip3 install jupyter-book "jupytext==1.7.1" pandas matplotlib "sphinxcontrib-bibtex<2.0.0"
RUN pip3 install --no-cache-dir --no-warn-script-location jupyter-book "jupytext==1.7.1" pandas matplotlib "sphinxcontrib-bibtex<2.0.0" numpy jupyter requests dash

RUN opam -y depext lwt_ssl
RUN opam install -y lwt_ssl merlin utop user-setup github-unix jupyter ocamlformat ocaml-lsp-server
RUN opam user-setup install

RUN sudo mkdir /usr/local/share/jupyter
RUN sudo chmod a+x /usr/local/share/jupyter
RUN /home/opam/.opam/4.10/bin/ocaml-jupyter-opam-genspec
RUN python3 /home/opam/.local/bin/jupyter kernelspec install --name ocaml-jupyter "$(opam var share)/jupyter" --user

# RISE
RUN pip3 install RISE
COPY .ocamlinit /home/opam/.ocamlinit
ENV PATH $PATH:/home/opam/.local/bin
RUN jupyter-nbextension enable rise --py

RUN ln -s /usr/bin/python3 /usr/bin/python & \
    ln -s /usr/bin/pip3 /usr/bin/pip

# Expose Jupyter port
EXPOSE 8888
