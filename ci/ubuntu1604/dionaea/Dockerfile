FROM ubuntu:16.04
ENV DEBIAN_FRONTEND noninteractive

# Speedup
RUN echo 'force-unsafe-io' | tee /etc/dpkg/dpkg.cfg.d/02apt-speedup && \
    echo 'DPkg::Post-Invoke {"/bin/rm -f /var/cache/apt/archives/*.deb || true";};' | tee /etc/apt/apt.conf.d/no-cache && \
    echo 'Acquire::http {No-Cache=True;};' | tee /etc/apt/apt.conf.d/no-http-cache

RUN apt-get update && \
    apt-get install -y \
        build-essential \
	check \
        cmake \
        cython3 \
        libcurl4-openssl-dev \
        libemu-dev \
        libev-dev \
        libglib2.0-dev \
        libloudmouth1-dev \
        libnetfilter-queue-dev \
        libnl-3-dev \
        libpcap-dev \
        libssl-dev \
        libtool \
        libudns-dev \
        python3 \
        python3-dev \
        python3-yaml \
        && \
    apt-get clean

COPY . /code
RUN cd /code && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_INSTALL_PREFIX:PATH=/opt/dionaea .. && \
    make && \
    make install

WORKDIR /opt/dionaea
VOLUME /opt/dionaea/var/log/dionaea
VOLUME /opt/dionaea/etc/dionaea
EXPOSE 21 69/udp 135 1433 1723 1883 3306 5060 5060/udp 5061 11211 27017

CMD ["/opt/dionaea/bin/dionaea","-c","/opt/dionaea/etc/dionaea/dionaea.cfg"]
