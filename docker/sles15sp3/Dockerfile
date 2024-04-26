FROM --platform=linux/amd64 registry.suse.com/bci/bci-base:15.5
WORKDIR /home

# Install dependencies
RUN zypper -n install gcc-c++
RUN zypper -n install make
RUN zypper -n install python311
RUN zypper -n install vim
RUN zypper -n install git

# Install rocm prereqs
RUN zypper -n --gpg-auto-import-keys addrepo https://download.opensuse.org/repositories/devel:/languages:/perl/15.5/devel:languages:perl.repo
RUN zypper -n --gpg-auto-import-keys install kernel-default-devel

# Register kernel-mode driver 
COPY amdgpu.repo /etc/zypp/repos.d
RUN zypper --no-gpg-checks --gpg-auto-import-keys -n ref

# Register rocm packages
COPY rocm.repo /etc/zypp/repos.d
RUN zypper --no-gpg-checks --gpg-auto-import-keys -n ref

# Install kernel driver and rocm packages
RUN zypper -n --gpg-auto-import-keys --no-gpg-checks install amdgpu-dkms
RUN zypper -n --gpg-auto-import-keys --no-gpg-checks install rocm6.0.1
RUN zypper -n --gpg-auto-import-keys --no-gpg-checks install rocm-hip-sdk5.7.2

    