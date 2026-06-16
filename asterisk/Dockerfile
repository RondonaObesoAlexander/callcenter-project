FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y \
    wget \
    gnupg2 \
    build-essential \
    libjansson-dev \
    libssl-dev \
    libxml2-dev \
    uuid-dev \
    libsqlite3-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Instalar Asterisk 18 LTS (estable)
WORKDIR /usr/src
RUN wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-18.12.1.tar.gz \
    && tar xzf asterisk-18.12.1.tar.gz \
    && cd asterisk-18.12.1 \
    && contrib/scripts/install_prereq install \
    && ./configure --libdir=/usr/lib \
    && make menuselect/menuselect --enable res_ari \
    && make menuselect/menuselect --enable res_ari_channels \
    && make menuselect/menuselect --enable res_ari_bridges \
    && make -j$(nproc) \
    && make install \
    && make config \
    && ldconfig

COPY ./etc/asterisk /etc/asterisk

EXPOSE 5060/udp 8088 5038

CMD ["asterisk", "-f", "-U", "root", "-G", "root"]