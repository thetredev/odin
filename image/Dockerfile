FROM ubuntu:22.04

ARG LLVM_VERSION=14
ARG ODIN_VERSION=dev-2023-05

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        gnupg2 \
        wget \
        curl \
        build-essential \
        cmake \
        python3 \
        libxml2-dev \
        git \
        lsb-release \
        software-properties-common \
    && wget -O /tmp/llvm.sh https://apt.llvm.org/llvm.sh \
    && chmod +x /tmp/llvm.sh \
    && /tmp/llvm.sh ${LLVM_VERSION} all \
    && rm -rf /tmp/llvm.sh \
    && apt-get clean \
    && rm -rf /var/cache/apt/lists/*

ENV PATH=/usr/lib/llvm-14/bin:${PATH}
ENV LLVM_CONFIG=llvm-config

RUN groupadd -rg 1000 odin \
    && useradd -ms /bin/bash -ru 1000 -g odin odin \
    && git clone -b ${ODIN_VERSION} --single-branch https://github.com/odin-lang/Odin.git /tmp/odin-git \
    && cd /tmp/odin-git \
    # apply workaround from https://github.com/odin-lang/Odin/issues/1839
    && sed -i s'/--libs core native --system-libs/--libs --system-libs/g' build_odin.sh \
    && make release_native \
    && cd ~ \
    && mkdir -p /opt/odin \
    && mv /tmp/odin-git/odin /opt/odin/odin \
    && mv /tmp/odin-git/core /opt/odin/core \
    && mv /tmp/odin-git/shared /opt/odin/shared \
    && mv /tmp/odin-git/vendor /opt/odin/vendor \
    && rm -rf /tmp/odin-git

ENV PATH=/opt/odin:${PATH}

USER odin
WORKDIR /home/odin
