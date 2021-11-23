FROM ubuntu:latest

RUN set -e && \
    apt-get update -q -y --no-install-recommends && \
    DEBIAN_FRONTEND="noninteractive" apt-get install -q -y --no-install-recommends \
        git \
        wget \
        build-essential \
        ca-certificates \
        cmake \
        automake \
        libtool \
        autoconf

WORKDIR /usr/src
RUN git clone https://github.com/xmrig/xmrig.git && \
    mkdir xmrig/build && \
    cd xmrig/scripts && \
    bash ./build_deps.sh && \
    cd ../build && \
    cmake .. -DXMRIG_DEPS=scripts/deps && \
    make -j$(nproc)

# ---

FROM ubuntu:latest

COPY --from=0 /usr/src/xmrig/build/xmrig /

RUN set -e && \
    apt-get update -q -y --no-install-recommends && \
    apt-get clean

WORKDIR /
ENTRYPOINT ["/xmrig"]
