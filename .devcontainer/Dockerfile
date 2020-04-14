FROM ubuntu:18.04

# Global settings
ENV CMAKE_VERSION=3.17.1
ENV CUDA_VERSION=10.2

RUN apt-get update \
    # Pre-requsities and development tools.
    && apt-get install -y software-properties-common unzip wget git build-essential gdb clang \
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
    # CUDA
    && wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin \
    && mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600 \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub \
    && add-apt-repository "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /" \
    && apt-get update \
    && apt-get -y install cuda-$CUDA_VERSION \
    #
    # Torch C++ (CUDA 10.1)
    && torch_version=1.4.0 \
    && wget https://download.pytorch.org/libtorch/cu101/libtorch-shared-with-deps-$torch_version.zip \
    && unzip libtorch-shared-with-deps-$torch_version.zip \
    && cp -r libtorch /usr/local/lib/ \
    #
    # Clean up
    && cd .. \
    && rm -r temp \
    && apt-get autoremove -y \
    && apt-get clean -y
