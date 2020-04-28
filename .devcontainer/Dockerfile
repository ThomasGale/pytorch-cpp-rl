FROM nvidia/cudagl:10.2-devel-ubuntu18.04

# Global settings
ENV CMAKE_VERSION=3.17.1
ENV TORCH_VERSION=1.5.0
# This is for CUDA 10.2
ENV CUDA_NN_VERSION=7.6.5

RUN apt-get update \
    # Pre-requsities and development tools.
    && apt-get install -y software-properties-common unzip wget git build-essential gdb clang python3-pip python-opengl libopenmpi-dev python3-dev zlib1g-dev libsm6 libxext6 libxrender-dev \
    #
    # Install CMake binary
    && mkdir -p ~/temp \
    && cd ~/temp \
    && wget https://github.com/Kitware/CMake/releases/download/v$CMAKE_VERSION/cmake-$CMAKE_VERSION-Linux-x86_64.sh \
    && mkdir /opt/cmake \
    && sh cmake-$CMAKE_VERSION-Linux-x86_64.sh --prefix=/opt/cmake --skip-license \
    && ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake \
    && cmake --version \
    #
    # GL - WIP
    && apt-get install -y mesa-utils \
    #
    # cuDNN - We need to use a mounted path with the downloaded tar file (you will need to download via nvidia login page)
    # TODO
    #
    # Torch C++ (CUDA) 
    && cd ~/temp && wget https://download.pytorch.org/libtorch/cu102/libtorch-shared-with-deps-$TORCH_VERSION.zip \
    && unzip libtorch-shared-with-deps-$TORCH_VERSION.zip \
    && cp -r libtorch /usr/local/lib/ \
    #
    # Python packages for OpenAI Gym, box2d environment (for LunarLander) and CPU tensorflow
    && pip3 install numpy 'gym[box2d]' tensorflow==1.14 zmq msgpack \
    && cd /usr/local && git clone https://github.com/openai/baselines.git \
    && cd baselines && pip3 install -e . \
    #
    # Clean up
    && rm -r ~/temp \
    && apt-get autoremove -y \
    && apt-get clean -y

# Configure a SUDO non-root user (no password)
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME