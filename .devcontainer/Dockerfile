FROM ubuntu:18.04

# Global settings
ENV CMAKE_VERSION=3.17.1
#ENV CUDA_VERSION=10.2

RUN apt-get update \
    # Pre-requsities and development tools.
    && apt-get install -y software-properties-common unzip wget git build-essential gdb clang python3-pip libopenmpi-dev python3-dev zlib1g-dev libsm6 libxext6 libxrender-dev \
    #
    # Install CMake binary
    && mkdir ~/temp \
    && cd ~/temp \
    && wget https://github.com/Kitware/CMake/releases/download/v$CMAKE_VERSION/cmake-$CMAKE_VERSION-Linux-x86_64.sh \
    && mkdir /opt/cmake \
    && sh cmake-$CMAKE_VERSION-Linux-x86_64.sh --prefix=/opt/cmake --skip-license \
    && ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake \
    && cmake --version \
    # 
    # CUDA - WIP
    #&& wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin \
    #&& mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600 \
    #&& apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub \
    #&& add-apt-repository "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /" \
    #&& apt-get update \
    #&& apt-get -y install cuda-$CUDA_VERSION \
    #&& \
    #&& \
    #
    # Torch C++ (CPU for now) 
    && torch_version=1.4.0 \
    && cd ~/temp && wget https://download.pytorch.org/libtorch/cpu/libtorch-shared-with-deps-$torch_version%2Bcpu.zip \
    && unzip libtorch-shared-with-deps-$torch_version+cpu.zip \
    && cp -r libtorch /usr/local/lib/ \
    #
    # Python packages for OpenAI Gym, box2d environment (for LunarLander) and CPU tensorflow
    && pip3 install numpy 'gym[box2d]' tensorflow==1.14 zmq msgpack \
    && cd ~ && git clone https://github.com/openai/baselines.git \
    && cd baselines && pip3 install -e . \
    #
    # Clean up
    && rm -r ~/temp \
    && apt-get autoremove -y \
    && apt-get clean -y
