FROM ubuntu:14.04
MAINTAINER kayhide "kayhide@gmail.com"

ENV HOME=/root

# Install dependencies.
RUN sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 575159689BEFB442
RUN echo 'deb http://download.fpcomplete.com/ubuntu yakkety main'| tee /etc/apt/sources.list.d/fpco.list
RUN sed -i.bak -e "s%http://archive.ubuntu.com/ubuntu/%http://ftp.jaist.ac.jp/pub/Linux/ubuntu/%g" /etc/apt/sources.list
RUN echo "deb http://us.archive.ubuntu.com/ubuntu trusty main multiverse" >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-get install -y --no-install-recommends \
        curl git htop man unzip vim wget \
        build-essential \
        cmake \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        libboost-thread-dev \
        cmake-curses-gui \
        libfftw3-dev \
        libfaac-dev \
        qt5-default qttools5-dev-tools \
        libgstreamer0.10-0 \
        libgstreamer-plugins-base0.10-0 \
        libopenblas-dev \
        libgtk-3-dev \
        liblapack-dev \
        liblapacke-dev \
        checkinstall \
        fakeroot

RUN apt-get install -y --no-install-recommends \
        postgresql-server-dev-all \
        postgresql-client-9.3 \
        postgresql-contrib \
        libpq-dev


# OpenCV
ENV OPENCV_ROOT=/opt/opencv
WORKDIR $OPENCV_ROOT

ENV CLONE_TAG=master

RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/jayrambhia/Install-OpenCV.git .
WORKDIR $OPENCV_ROOT/Ubuntu
RUN chmod +x *
RUN ./opencv_latest.sh


# Caffe
ENV CAFFE_ROOT=/opt/caffe
WORKDIR $CAFFE_ROOT

ENV CLONE_TAG=master

RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/BVLC/caffe.git .
RUN mkdir build_cpu && cd build_cpu && \
    cmake -DCPU_ONLY=ON -DBUILD_python=OFF .. && \
    make -j"$(nproc)" && \
    make install


# Stack
WORKDIR $HOME

RUN curl -sSL https://get.haskellstack.org/ | sh
RUN stack upgrade


# Node.js
WORKDIR $HOME

RUN apt-get install -y nodejs npm
RUN npm cache clean
RUN npm install n -g
RUN n stable
RUN ln -sf /usr/local/bin/node /usr/bin/node
RUN apt-get purge -y nodejs npm
RUN npm install --unsafe-perm -g elm
