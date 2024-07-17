# Image: stateoftheartio/qt6:6.6-wasm-aqt

FROM ubuntu:24.04
MAINTAINER State Of The Art <docker@state-of-the-art.io> (@stateoftheartio)

# PIP requirement like "aqtinstall==2.0.6" or url with egg file
## Tim Angus's aqtinstall fork with wasm support
#ARG AQT_VERSION="git+https://github.com/timangus/aqtinstall"
# AlTeveDev's aqtinstall fork with wasm support
ARG AQT_VERSION="git+https://github.com/AlTeveDev/aqtinstall"

ARG QT_VERSION=6.7.1
ARG QT_PATH=/opt/Qt

ARG SDK_PATH=/opt/emsdk
ARG SDK_VERSION=3.1.50
ARG SDK_SHA256=7491a881eb5ee15fe81bbabcfff1fd571e45ccdb24a81890af429f9970cbd1f3

ARG ADDITIONAL_PACKAGES="sudo git openssh-client ca-certificates curl python3 locales ninja-build"

ENV DEBIAN_FRONTEND=noninteractive \
    DEBCONF_NONINTERACTIVE_SEEN=true \
    QT_VERSION=${QT_VERSION} \
    AQT_VERSION=${AQT_VERSION} \
    QT_PATH=${QT_PATH} \
    QT_WASM=${QT_PATH}/${QT_VERSION}/wasm_singlethread \
    EMSDK=${SDK_PATH} \
    EMSDK_NODE=${SDK_PATH}/node/15.14.0_64bit/bin/node \
    PATH=$PATH:${QT_PATH}/Tools/CMake/bin:${QT_PATH}/Tools/Ninja:${QT_PATH}/${QT_VERSION}/wasm_singlethread/bin:${SDK_PATH}/upstream/bin:${SDK_PATH}/upstream/emscripten:${SDK_PATH}/node/15.14.0_64bit/bin:${SDK_PATH}

COPY get_emsdk.sh get_qt.sh install_packages.sh /tmp/

# Install emscripten
RUN /tmp/get_emsdk.sh

# Get Qt binaries with aqt
RUN /tmp/get_qt.sh

# Install the required packages
RUN /tmp/install_packages.sh

# Reconfigure locale
RUN locale-gen en_US.UTF-8 && dpkg-reconfigure locales

# Add group & user + sudo
RUN groupadd -r user && useradd --create-home --gid user user && echo 'user ALL=NOPASSWD: ALL' > /etc/sudoers.d/user

USER user
WORKDIR /home/user
ENV HOME /home/user
