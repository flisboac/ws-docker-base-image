FROM ubuntu:artful

ENV DEBIAN_FRONTEND="teletype"

# Update system and install essential developer tools
RUN apt-get update \
    && apt-get -y install \
    software-properties-common \
    curl \
    sudo \
    apt-transport-https \
    git \
    subversion \
    snapd \
    bash

# Install Developer fonts and additional tools
RUN apt-get install -y \
    fonts-hack-ttf \
    fonts-firacode
