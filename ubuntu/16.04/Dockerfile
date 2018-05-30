FROM ubuntu:16.04

LABEL maintainer="Oleg Kulyk" \
      description="Ubuntu 16.04, development base image" \
      run="docker run -it --rm -v $(pwd):/home/phantom/app eleidan/base:ubuntu-16.04" \
      version="2018-05-30"

ARG CUSTOM_USER="phantom"
ENV SERVICE_NAME="base:ubuntu-16.04" \
    GOSU_VERSION="1.10"
ENV CONTAINER_PS1="[ $(get_container_ip) | \[\e[0;36m\]$(get_container_service)\[\e[m\] | \[\e[0;35m\]\w\[\e[m\] ]\n\[\e[1;32m\]>\[\e[m\] "


COPY .profile .bashrc .bash_history /etc/skel/
COPY .profile .bashrc .bash_history /root/

COPY docker-entrypoint.sh /

RUN apt-get update && apt-get install -y --no-install-recommends \
        autoconf \
        automake \
        bash-completion \
        bzip2 \
        ca-certificates \
        curl \
        file \
        g++ \
        gcc \
        imagemagick \
        libbz2-dev \
        libc6-dev \
        libcurl4-openssl-dev \
        libdb-dev \
        libevent-dev \
        libffi-dev \
        libgeoip-dev \
        libglib2.0-dev \
        libjpeg-dev \
        liblzma-dev \
        libmagickcore-dev \
        libmagickwand-dev \
        libncurses-dev \
        libpng-dev \
        libpq-dev \
        libreadline-dev \
        libsqlite3-dev \
        libssl-dev \
        libtool \
        libwebp-dev \
        libxml2-dev \
        libxslt-dev \
        libyaml-dev \
        make \
        patch \
        wget \
        xz-utils \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    if ! command -v gpg > /dev/null; then \
      apt-get update; \
      apt-get install -y --no-install-recommends \
        gnupg2 \
        dirmngr \
      ; \
      rm -rf /var/lib/apt/lists/*; \
    fi


################################################################################
# SCM
################################################################################
RUN apt-get update && apt-get install -y --no-install-recommends \
        bzr \
        git \
        mercurial \
        openssh-client \
        subversion \
        \
        procps \
&& rm -rf /var/lib/apt/lists/*


################################################################################
# GOSU
################################################################################
RUN dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true \
    && useradd -m -s /bin/bash -u 1000 $CUSTOM_USER \
    && chmod a+x /docker-entrypoint.sh


ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["/bin/bash"]
