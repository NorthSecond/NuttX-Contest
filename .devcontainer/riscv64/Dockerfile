# dockerfile for devcontainer
# Nuttx development environment in vscode dev-container
# From Ubuntu 22.04
# use qemu to emulate RISC-V 64

FROM ubuntu:22.04

LABEL maintainer="Yifei Yang <yangyf83@aliyun.com>"

# if need change apt source
# RUN sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list && sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list && 

# if need change the apt source to tuna
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
    sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list

# Install packages
RUN apt-get update && \
    apt-get install -y autoconf automake autotools-dev curl python3 libmpc-dev wget\
    libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool \
    patchutils bc zlib1g-dev libexpat-dev ninja-build git net-tools kconfig-frontends \
    gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu \
    pkg-config  libglib2.0-dev libpixman-1-dev libsdl2-dev tmux python3-pip vim genromfs libncurses5

# Install riscv-gnu-toolchain
RUN wget https://mirror.iscas.ac.cn/riscv-toolchains/release/riscv-collab/riscv-gnu-toolchain/LatestRelease/riscv64-elf-ubuntu-22.04-nightly-2023.04.18-nightly.tar.gz && riscv64-elf-ubuntu-22.04-nightly-2023.04.18-nightly.tar.gz && echo "export PATH=/riscv/bin:$PATH" >> ~/.bashrc && rm -rf riscv64-elf-ubuntu-22.04-nightly-2023.04.18-nightly.tar.gz

# download another version
RUN wget https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz

# add to path
ENV PATH="riscv64/bin:${PATH}"

ENV TZ=Asia/Shanghai

# Install qemu for riscv64 version 7.1.0
RUN wget https://download.qemu.org/qemu-7.1.0.tar.xz && \
    tar xvJf qemu-7.1.0.tar.xz && cd qemu-7.1.0 && \
    ./configure --target-list=riscv64-softmmu,riscv32-softmmu,riscv64-linux-user &&  \
    make -j8 && make -j$(nproc) && make install && cd .. && rm -rf qemu-7.1.0*

CMD ["/bin/bash"]
