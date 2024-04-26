FROM --platform=linux/amd64 ubuntu:focal
WORKDIR /home

USER root

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ "US/Chicago"

RUN apt update

# Install python3.11
RUN apt install -y software-properties-common && \
    apt install -y build-essential && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt update && \
    apt install -y python3.11 python3.11-venv python3.11-dev

# Install dependencies
RUN apt install -y cmake
RUN apt install -y wget
RUN apt install -y gpg
RUN apt install -y git
RUN apt install -y vim

# Install rocm prereqs
RUN apt install -y "linux-headers-$(uname -r)" "linux-modules-extra-$(uname -r)"
RUN mkdir --parents --mode=0755 /etc/apt/keyrings
RUN wget https://repo.radeon.com/rocm/rocm.gpg.key -O - | \
gpg --dearmor | tee /etc/apt/keyrings/rocm.gpg > /dev/null

# Register kernel-mode driver 
COPY amdgpu.list /etc/apt/sources.list.d
RUN apt update

# Register rocm packages
COPY rocm.list /etc/apt/sources.list.d
RUN echo 'Package: *' > /etc/apt/preferences.d/rocm-pin-600 && \
    echo 'Pin: release o=repo.radeon.com' >> /etc/apt/preferences.d/rocm-pin-600 && \
    echo 'Pin-Priority: 600' >> /etc/apt/preferences.d/rocm-pin-600
RUN apt update

# Install kernel driver and rocm packages
RUN apt install -y amdgpu-dkms
RUN apt install -y rocm6.0.1
RUN apt install -y rocm-hip-sdk5.7.2
