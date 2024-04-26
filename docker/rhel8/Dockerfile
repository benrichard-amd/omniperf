FROM --platform=linux/amd64 rockylinux:8.8
WORKDIR /home

# Install dependencies
RUN yum -y install wget
RUN yum -y install cmake
RUN yum -y install gcc-c++
RUN yum -y install python311

# Install rocm prereqs
RUN wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
RUN rpm -ivh --replacefiles epel-release-latest-8.noarch.rpm
RUN dnf -y install dnf-plugin-config-manager
RUN crb enable
RUN yum -y install kernel-headers kernel-devel

# Register kernel-mode driver 
COPY amdgpu.repo /etc/yum.repos.d
RUN yum clean all

# Register rocm packages
COPY rocm.repo /etc/yum.repos.d
RUN yum clean all

# Install kernel driver and rocm packages
RUN yum -y install amdgpu-dkms
RUN yum -y install rocm6.0.1
RUN yum -y install rocm-hip-sdk5.7.2