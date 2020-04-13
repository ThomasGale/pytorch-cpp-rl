FROM ubuntu:18.04

RUN apt-get update \
    # Pre-requsities
    && apt-get install -y unzip wget git build-essential gdb \
    #
    # Install CMake binary
    && cmake_version=3.17.1 \
    && mkdir ~/temp \
    && cd ~/temp \
    && wget https://github.com/Kitware/CMake/releases/download/v$cmake_version/cmake-$cmake_version-Linux-x86_64.sh \
    && mkdir /opt/cmake \
    && sh cmake-$cmake_version-Linux-x86_64.sh --prefix=/opt/cmake --skip-license \
    && ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake \
    && cmake --version \
    #
    # C++
    && apt-get install -y clang \
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
