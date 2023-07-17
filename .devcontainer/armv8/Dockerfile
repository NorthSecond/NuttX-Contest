# dockerfile for devcontainer
# Nuttx development environment in vscode dev-container
# From Ubuntu 18.04 due to some linking errors in Ubuntu 22.04 (because some packages's version is too new for Nuttx)
# use qemu to emulate armv8

FROM ubuntu:18.04

LABEL maintainer="Yifei Yang <yangyf83@aliyun.com>"

# Codespace Doesn't need to change sources.
# RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
#     sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
#     sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list 
    # sed -i 's/http:/https:/g' /etc/apt/sources.list
    # sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    # sed -i 's/security.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list

# Install packages
RUN apt-get update && \
    apt-get install -y qemu-system-arm qemu-efi-aarch64 qemu-utils wget xz-utils \
    git vim tmux build-essential flex bison automake autoconf libtool python3-pip \
    python3-setuptools gettext texinfo libncurses5-dev libncursesw5-dev xxd \
    gperf pkg-config genromfs libgmp-dev libmpc-dev libmpfr-dev libisl-dev \
    binutils-dev libelf-dev libexpat-dev gcc-multilib g++-multilib picocom \
    u-boot-tools util-linux lsof curl net-tools && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# The Kconfig-frontends package is required for the NuttX configuration tools
# BUT the version in the Ubuntu repository is too old.  So we will build it from source.
RUN git clone https://bitbucket.org/nuttx/tools.git && \
    cd tools/kconfig-frontends && \
    ./configure --enable-mconf --disable-nconf --disable-gconf --disable-qconf && \
    make -j && make install && cd ../.. && rm -rf tools

# Install cross compiler
RUN wget https://developer.arm.com/-/media/Files/downloads/gnu/11.2-2022.02/binrel/gcc-arm-11.2-2022.02-x86_64-aarch64-none-elf.tar.xz && \
    xz -d gcc-arm-11.2-2022.02-x86_64-aarch64-none-elf.tar.xz && \
    tar xf gcc-arm-11.2-2022.02-x86_64-aarch64-none-elf.tar && \
    echo "export PATH=$PATH:/gcc-arm-11.2-2022.02-x86_64-aarch64-none-elf/bin" >> ~/.bashrc

ENV PATH="/gcc-arm-11.2-2022.02-x86_64-aarch64-none-elf/bin:${PATH}"
ENV LD_LIBRARY_PATH="/usr/local/lib:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/usr/local/lib64:${LD_LIBRARY_PATH}"
ENV TZ Asia/Shanghai

CMD ["/bin/bash"]
