##
# symbioquine/map-proxy-docker

# This file is available at the option of the licensee under:
# Public domain
# or licensed under X/MIT (LICENSE.TXT) Copyright 2019 Symbioquine <symbioquine@gmail.com>

FROM osgeo/gdal:alpine-normal-latest as builder

MAINTAINER Symbioquine <symbioquine@gmail.com>

# Based on https://github.com/gsemet/docker-alpine-s6-python3-twisted/blob/master/Dockerfile
RUN \
apk add --no-cache --virtual=build-dependencies \
    autoconf \
    automake \
    freetype-dev \
    g++ \
    gcc \
    jpeg-dev \
    lcms2-dev \
    libffi-dev \
    libpng-dev \
    libwebp-dev \
    libxml2-dev \
    libxslt-dev \
    linux-headers \
    make \
    openjpeg-dev \
    openssl-dev \
    python3-dev \
    tiff-dev \
    zlib-dev && \

apk add --no-cache --virtual=run-dependencies \
    libxml2 \
    libxslt && \

pip3 install --no-cache-dir -U \
    'lxml' \
    'pyopenssl' \
    'service_identity'

RUN \
pip3 install --no-cache-dir -U 'MapProxy==1.12.0' && \

# clean up
apk del --purge \
    build-dependencies && \
rm -rf \
    /root/.cache \
    /tmp/*

COPY . /app
WORKDIR /app

ENTRYPOINT ["mapproxy-util"]
