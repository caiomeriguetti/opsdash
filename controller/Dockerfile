FROM phusion/baseimage:latest

MAINTAINER Cristian Mello <cristianc.mello@gmail.com>

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8
ENV TERM xterm

RUN DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common \
    make \
    automake \
    gcc \
    build-essential \
    g++ \
    cpp \
    libc6-dev \
    man-db \
    autoconf \
    pkg-config \
    libcurl4-openssl-dev \
    libedit-dev \
    libssl-dev \
    libxml2-dev \
    xz-utils \
    git \
    curl \
    vim \
    nano \
    python-pip \
    net-tools \
    python-dev \
    python-crypto

#####################################
# Non-Root User:
#####################################

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ARG PGID=1000
RUN groupadd -g $PGID picpayops && \
    useradd -u $PUID -g picpayops -m picpayops

# Define password for picpayops user
RUN echo picpayops:123456 | /usr/sbin/chpasswd

#####################################
# ssh:
#####################################

ADD insecure_id_rsa /tmp/id_rsa
ADD insecure_id_rsa.pub /tmp/id_rsa.pub

RUN rm -f /etc/service/sshd/down && \
    cat /tmp/id_rsa.pub >> /root/.ssh/authorized_keys \
        && cat /tmp/id_rsa.pub >> /root/.ssh/id_rsa.pub \
        && cat /tmp/id_rsa >> /root/.ssh/id_rsa \
        && rm -f /tmp/id_rsa* \
        && chmod 644 /root/.ssh/authorized_keys /root/.ssh/id_rsa.pub \
    && chmod 400 /root/.ssh/id_rsa
    
    
#####################################
# Python Fabric
#####################################

RUN pip install --upgrade pip \
	&& pip install setuptools \
	&& pip install paramiko \
	&& pip install fabric
    
# Set default work directory
WORKDIR /var/www/workspace

