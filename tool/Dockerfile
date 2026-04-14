FROM archlinux:latest

RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm base-devel gmp mpfr libmpc wget sed coreutils

ENV PREFIX="/opt/toolchain"
ENV TARGET=i686-elf
ENV PATH="${PREFIX}/bin:${PATH}"
WORKDIR /build-tmp

RUN wget https://ftp.gnu.org/gnu/binutils/binutils-2.41.tar.gz && \
    tar -xf binutils-2.41.tar.gz && \
    mkdir binutils-build && cd binutils-build && \
    ../binutils-2.41/configure --target=$TARGET --prefix="$PREFIX" --with-sysroot --disable-nls --disable-werror && \
    make -j$(nproc) && \
    make install && \
    cd .. && rm -rf binutils*

RUN wget https://ftp.gnu.org/gnu/gcc/gcc-13.2.0/gcc-13.2.0.tar.gz && \
    tar -xf gcc-13.2.0.tar.gz && \
    mkdir gcc-build && cd gcc-build && \
    ../gcc-13.2.0/configure --target=$TARGET --prefix="$PREFIX" --disable-nls --enable-languages=c,c++ --without-headers && \
    make -j$(nproc) all-gcc && \
    make -j$(nproc) all-target-libgcc && \
    make install-gcc && \
    make install-target-libgcc && \
    cd .. && rm -rf gcc*

RUN rm -rf /build-tmp

WORKDIR /root/os
CMD ["/bin/bash"]
