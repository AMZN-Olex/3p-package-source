#
# Copyright (c) Contributors to the Open 3D Engine Project.
# For complete copyright and license terms please see the LICENSE at the root of this distribution.
#
# SPDX-License-Identifier: Apache-2.0 OR MIT
#

# This docker file uses ubuntu 22.04 as the base image so that the AWS Native C++ libraries will use OpenSSL 3 as the base
# for its dependencies
#


ARG INPUT_ARCHITECTURE=amd64
ARG INPUT_IMAGE=ubuntu:20.04
ARG INPUT_DOCKER_BUILD_SCRIPT

FROM ${INPUT_ARCHITECTURE}/${INPUT_IMAGE}

ARG INPUT_DOCKER_BUILD_SCRIPT

WORKDIR /data/workspace

# Initilize apt cache
RUN apt-get clean && apt-get update

# Setup time zone and locale data (necessary for SSL and HTTPS packages)
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata locales keyboard-configuration

# Install the development packages needed to build AWS Gamelift
RUN apt-get install -y build-essential \
                       git \
                       cmake \
                       python3 \
                       ninja-build \
                       libssl-dev

# Prevent the copying of the src folder from being cached
ARG CACHEBUST=1

RUN cd /data/workspace && \
    mkdir src

# Copy the git synced source from the context base to this container
COPY src  /data/workspace/src/

# Copy the build script specific to this Docker script in order to execute the build
COPY ${INPUT_DOCKER_BUILD_SCRIPT} /data/workspace/



