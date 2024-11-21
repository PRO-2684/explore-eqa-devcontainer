FROM nvidia/cudagl:11.3.0-devel-ubuntu20.04

# Install some basic utilities
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libopengl0 freeglut3-dev wget

# Copy environment.yml (if found) to a temp location so we update the environment. Also
# copy "noop.txt" so the COPY instruction does not fail if no environment.yml exists.
COPY environment.yml* .devcontainer/noop.txt /tmp/conda-tmp/

# Install miniconda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh \
    && /bin/bash /tmp/miniconda.sh -b -p /opt/conda \
    && rm /tmp/miniconda.sh
ENV PATH /opt/conda/bin:$PATH
RUN conda init

# Update the environment if the environment.yml file exists
RUN if [ -f "/tmp/conda-tmp/environment.yml" ]; then umask 0002 && /opt/conda/bin/conda env update -n base -f /tmp/conda-tmp/environment.yml; fi \
    && rm -rf /tmp/conda-tmp

# Install habitat-sim
RUN conda install -y habitat-sim headless -c conda-forge -c aihabitat

# Set environment variables
ENV HF_HOME /workspaces/explore-eqa/data/huggingface

# Workspace setup (Before building the dev container):
# Clone target repository: `git clone https://github.com/Stanford-ILIAD/explore-eqa.git`.
# Download and extract `hm3d-train-habitat-v0.2.tar` to `explore-eqa/data/hm3d-train-habitat-v0.2`, as documented [here](https://github.com/matterport/habitat-matterport-3dresearch#-downloading-hm3d-v02:~:text=32G-,hm3d%2Dtrain%2Dhabitat%2Dv0.2.tar,-train).
# Put your HF token with access to the models at `explore-eqa/data/huggingface/token`.
# Adjust `cfg/*` files as needed.
# - Set `scene_data_path` to `data/hm3d-train-habitat-v0.2`.
# - Set `hf_token` (if present).

# # Please execute (After building the dev container):
# conda init --all
# # Restart the shell
# conda activate base && pip install -e . && cd prismatic-vlms && pip install -e . && cd ..
# cd CLIP && pip install -e . && cd .. # For running CLIP-based exploration
