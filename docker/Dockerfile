# Prepare the image with:
#   docker build -t gnezdo/tfhs:2016-10-14-1 docker
FROM gcr.io/tensorflow/tensorflow:latest-devel
MAINTAINER Greg Steuck <gnezdo+tfhs@google.com>
# Installs protoc and the libraries.
RUN \
    cd /tensorflow && \
    bazel --batch build -c opt '@protobuf//:protoc' && \
    install -s bazel-bin/external/protobuf/protoc /usr/local/bin && \
    bazel --batch build -c opt '//tensorflow:libtensorflow_c.so' && \
    install bazel-bin/tensorflow/libtensorflow_c.so /usr/local/lib && \
    bazel --batch clean

RUN apt-get update

RUN apt-get install -y \
        # Avoids /usr/bin/ld: cannot find -ltinfo
        libncurses5-dev \
        # Makes stack viable in the container
        libgmp-dev \
        # Required for locales configuration.
        locales

# Our MNIST demo program outputs Unicode characters.
RUN dpkg-reconfigure locales && \
    locale-gen en_US.UTF-8 && \
    update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8
