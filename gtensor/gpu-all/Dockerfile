# Can be used for local gtensor dev, too big for CI

FROM docker.io/rocm/dev-ubuntu-20.04:4.1.1-complete

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update && apt-get install -y --no-install-recommends \
  ca-certificates \
  gnupg \
  git \
  build-essential \
  wget

# add apt repo public key
ARG url=https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB
ADD $url /
RUN file=$(basename "$url") && \
    apt-key add "$file" && \
    rm "$file"

# configure the repository
ARG oneapi_repo=https://apt.repos.intel.com/oneapi
RUN echo "deb $oneapi_repo all main" > /etc/apt/sources.list.d/oneAPI.list

ARG graphics_repo=https://repositories.intel.com/graphics/ubuntu
RUN echo "deb [trusted=yes arch=amd64] $graphics_repo bionic-devel main" > /etc/apt/sources.list.d/intel-graphics.list

# install cmake
ARG cmake_url=https://github.com/Kitware/CMake/releases/download/v3.20.0/cmake-3.20.0-linux-x86_64.sh
ADD $cmake_url /
RUN file=$(basename "$cmake_url") && \
    bash $file --prefix=/usr --skip-license && \
    rm $file

RUN apt-get update -y
RUN apt-get install -y --no-install-recommends \
  intel-oneapi-compiler-dpcpp-cpp \
  intel-oneapi-mkl \
  intel-oneapi-mkl-devel \
  clinfo \
  intel-opencl-icd \
  intel-level-zero-gpu \
  level-zero \
  nvidia-cuda-toolkit

RUN apt-get clean \
 && rm -rf /var/lib/apt/lists/*
