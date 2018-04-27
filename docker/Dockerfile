FROM ubuntu:16.04
MAINTAINER kayhide "kayhide@gmail.com"

# Install dependencies.
RUN sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 575159689BEFB442 \
    && echo 'deb http://download.fpcomplete.com/ubuntu yakkety main'| tee /etc/apt/sources.list.d/fpco.list \
    && sed -i.bak -e "s%http://archive.ubuntu.com/ubuntu/%http://ftp.jaist.ac.jp/pub/Linux/ubuntu/%g" /etc/apt/sources.list \
    && echo "deb http://us.archive.ubuntu.com/ubuntu trusty main multiverse" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y \
       build-essential checkinstall cmake pkg-config yasm \
       curl wget git man \
       unzip \
       sudo \
       fakeroot \
       stack \
       nodejs npm \
       vim

# Postgresql packages
RUN apt-get install -y --no-install-recommends \
    postgresql-server-dev-all \
    postgresql-client-9.5 \
    postgresql-contrib \
    libpq-dev

# Caffe related packages
# http://caffe.berkeleyvision.org/install_apt.html
RUN apt-get install -y --no-install-recommends libboost-all-dev
RUN apt-get install -y \
    libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler \
    libatlas-base-dev libopenblas-dev \
    liblmdb-dev \
    libgflags-dev \
    libgoogle-glog-dev


# Caffe
ENV CAFFE_ROOT=/opt/caffe
WORKDIR $CAFFE_ROOT
ENV CLONE_TAG=1.0
RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/BVLC/caffe.git . \
    && mkdir build && cd build \
    && cmake -DCPU_ONLY=ON -DBUILD_python=OFF .. \
    && make && make install \
    && rm -rf src \
    && cd .. && rm -rf .git src python


# Stack
RUN cd
RUN stack upgrade
ENV PATH=/root/.local/bin:$PATH
RUN stack update \
    && stack --resolver lts-10.1 setup


# Node.js
RUN cd
RUN npm cache clean \
    && npm install n -g \
    && n stable \
    && ln -sf /usr/local/bin/node /usr/bin/node


# Elm
RUN cd
RUN npm install --unsafe-perm -g elm
