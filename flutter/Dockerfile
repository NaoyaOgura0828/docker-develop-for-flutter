# Set the base image
ARG BASE_IMAGE_TAG=${BASE_IMAGE_TAG}
FROM ubuntu:${BASE_IMAGE_TAG}

# Set ARG
ARG TZ=Asia/Tokyo
ARG USER_NAME=${USER_NAME}
ARG FLUTTER_VERSION=${FLUTTER_VERSION}
ARG ANDROID_COMMAND_LINE_TOOLS_VERSION=${ANDROID_COMMAND_LINE_TOOLS_VERSION}
ARG ANDROID_API_LEVEL=${ANDROID_API_LEVEL}
ARG ANDROID_SDK_BUILD_TOOLS_VERSION=${ANDROID_SDK_BUILD_TOOLS_VERSION}

# Set TimeZone
RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

# Repository Update
RUN apt-get update -y &&\
    apt-get upgrade -y

# Install Packages
RUN apt-get install curl -y &&\
    apt-get install git -y &&\
    apt-get install gnupg2 -y &&\
    apt-get install libcanberra-gtk-module -y &&\
    apt-get install libcanberra-gtk3-module -y &&\
    apt-get install locales -y &&\
    apt-get install packagekit-gtk3-module -y &&\
    apt-get install software-properties-common -y &&\
    apt-get install sudo -y &&\
    apt-get install unzip -y &&\
    apt-get install vim -y &&\
    apt-get install wget -y &&\
    apt-get install zip -y

# Set Env
RUN locale-gen ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja

# Install Dart
RUN apt-get install apt-transport-https -y &&\
    wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/dart.gpg &&\
    echo "deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main" | sudo tee /etc/apt/sources.list.d/dart_stable.list &&\
    apt-get update -y &&\
    apt-get install dart -y

# Install Flutter Required Packages
RUN apt-get install clang -y &&\
    apt-get install cmake -y &&\
    apt-get install ninja-build -y &&\
    apt-get install pkg-config -y &&\
    apt-get install openjdk-17-jdk -y &&\
    apt-get install libgtk-3-dev -y

# Add User
RUN adduser ${USER_NAME} --force-badname

# Setup to use sudo without password
RUN echo "${USER_NAME} ALL=NOPASSWD: ALL" >> /etc/sudoers

# Setup WORKDIR
WORKDIR /home/${USER_NAME}

# Change User
USER ${USER_NAME}

# Set ENV
ENV PATH=$PATH:/usr/lib/dart/bin
ENV PATH=/home/${USER_NAME}/.pub-cache/bin:$PATH
ENV FVM_HOME=/home/${USER_NAME}/.fvm
ENV PATH $FVM_HOME/flutter_sdk/bin:$PATH
ENV ANDROID_HOME=/usr/lib/android-sdk
ENV PATH $PATH:${ANDROID_HOME}/cmdline-tools/latest/bin
ENV PATH=$PATH:${ANDROID_HOME}/platform-tools
ENV PATH=$PATH:${ANDROID_HOME}/build-tools/${ANDROID_SDK_BUILD_TOOLS_VERSION}

# Install Flutter
RUN dart pub global activate melos --verbose && \
    dart pub global activate fvm --verbose && \
    chmod 700 /home/${USER_NAME}/.pub-cache/bin/fvm && \
    fvm doctor --verbose && \
    fvm install ${FLUTTER_VERSION} --verbose && \
    fvm use --force ${FLUTTER_VERSION} --verbose && \
    fvm flutter config --enable-web --enable-linux-desktop --enable-macos-desktop --enable-windows-desktop --enable-android --enable-ios --enable-fuchsia && \
    fvm flutter precache --verbose && \
    fvm flutter doctor --verbose

# Change root
USER root

# Install cmdline-tools
RUN wget -P /tmp https://dl.google.com/android/repository/${ANDROID_COMMAND_LINE_TOOLS_VERSION} && \
    mkdir -p /usr/lib/android-sdk/cmdline-tools/ && \
    unzip /tmp/${ANDROID_COMMAND_LINE_TOOLS_VERSION} -d /usr/lib/android-sdk/cmdline-tools/ && \
    mv /usr/lib/android-sdk/cmdline-tools/cmdline-tools /usr/lib/android-sdk/cmdline-tools/latest

# All SDK package licenses accept
RUN yes | sdkmanager --licenses

# Install platform-tools
RUN sdkmanager platform-tools

# Install platforms
RUN sdkmanager "platforms;android-${ANDROID_API_LEVEL}"

# Install build-tools
RUN sdkmanager "build-tools;${ANDROID_SDK_BUILD_TOOLS_VERSION}"
