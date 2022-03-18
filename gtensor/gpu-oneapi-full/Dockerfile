# Used for gtesor CI, push to ghcr.io/wdmapp/oneapi-dpcpp-ubuntu-20.04:latest
FROM ghcr.io/wdmapp/oneapi-dpcpp-ubuntu-20.04:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y
&& apt-get install -y --no-install-recommends \
  intel-oneapi-compiler-fortran \
  intel-oneapi-runtime-fortran \
  intel-oneapi-runtime-mkl \
  intel-oneapi-mpi \
  intel-oneapi-mpi-devel

RUN apt-get clean \
 && rm -rf /var/lib/apt/lists/*
