FROM ubuntu:focal

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get -yq dist-upgrade && \
    apt-get install -y --no-install-recommends \
        python3-pip \
        python3-setuptools \
        python3-wheel \
        ninja-build \
        build-essential \
        flex \
        bison \
        git \
        cmake \
        meson \
        libsctp-dev \
        libgnutls28-dev \
        libgcrypt-dev \
        libssl-dev \
        libidn11-dev \
        libmongoc-dev \
        libbson-dev \
        libyaml-dev \
        libmicrohttpd-dev \
        libcurl4-gnutls-dev \
        libnghttp2-dev \
        libtalloc-dev \
        iproute2 \
        ca-certificates \
        netbase \
        iptables \
        dnsutils \
        net-tools \
        iputils-ping \
        tshark \
        tcpdump \
        ufw \
        curl \
        wget \
        gnupg \
        vim \
        less \
        pkg-config && \
    apt-get clean 

RUN git clone --recursive https://github.com/open5gs/open5gs \
    && cd open5gs \
    && git checkout v2.6.6 \
    && meson build --prefix=`pwd`/install \
    && ninja -C build \
    && cd build \
    && ninja install

WORKDIR /

CMD ["sh", "-c", "tail -f /dev/null"]
