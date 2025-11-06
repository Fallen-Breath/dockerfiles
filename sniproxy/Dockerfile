FROM ubuntu:24.04 AS builder

RUN <<EOT
set -eux
apt update
apt install -y autotools-dev cdbs debhelper dh-autoreconf dpkg-dev gettext libev-dev libpcre2-dev libudns-dev pkg-config fakeroot devscripts unzip wget

mkdir /build
cd /build
wget https://github.com/dlundquist/sniproxy/archive/refs/tags/0.7.0.zip
unzip 0.7.0.zip
cd sniproxy-0.7.0

./autogen.sh && dpkg-buildpackage
EOT

FROM ubuntu:24.04

COPY --from=builder /build/sniproxy_0.7.0_amd64.deb /sniproxy_0.7.0_amd64.deb
RUN <<EOT
set -eux
apt update
apt install -y libudns0t64 libev4t64

dpkg -i /sniproxy_0.7.0_amd64.deb
rm -f /sniproxy_0.7.0_amd64.deb
rm -rf /var/lib/apt/lists/*
EOT

ENTRYPOINT ["sniproxy"]
