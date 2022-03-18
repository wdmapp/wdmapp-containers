# Used for gtesor CI, push to ghcr.io/wdmapp/oneapi-dpcpp-ubuntu-20.04:latest
FROM docker.io/ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update \
    && apt-get install -y --no-install-recommends locales \
      ca-certificates \
      gnupg \
      git \
      build-essential \
      wget \
      clang-9
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && update-locale LANG=en_US.UTF-8
ENV LANG en_US.UTF-8 
ENV LC_ALL en_US.UTF-8

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
RUN echo "deb [trusted=yes arch=amd64] $graphics_repo focal main" > /etc/apt/sources.list.d/intel-graphics.list

# install cmake
ARG cmake_url=https://github.com/Kitware/CMake/releases/download/v3.20.6/cmake-3.20.6-linux-x86_64.sh
ADD $cmake_url /
RUN file=$(basename "$cmake_url") && \
    bash $file --prefix=/usr --skip-license && \
    rm $file

RUN apt-get update -y
 && apt-get install -y --no-install-recommends \
  intel-oneapi-compiler-dpcpp-cpp \
  intel-oneapi-mkl \
  intel-oneapi-mkl-devel \
  clinfo \
  intel-opencl-icd \
  intel-level-zero-gpu \
  level-zero \
  level-zero-dev

RUN apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# setvars.sh environment variables
RUN env > default_env_vars; \
. /opt/intel/oneapi/setvars.sh; \
env > env_vars; \
diff default_env_vars env_vars \
| grep ">" | sed  s/..// \
| sort \
| sed 's/^/export /' | sed "s/=/='/" | sed "s/$/'/" \
>> /root/.oneapi_env_vars; \
# clean up
rm *env_vars

ENTRYPOINT ["bash", "-c", "source /root/.oneapi_env_vars && \"$@\"", "bash"]
