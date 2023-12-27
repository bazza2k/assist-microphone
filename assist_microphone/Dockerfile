ARG BUILD_FROM
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

WORKDIR /usr/src
ARG HOMEASSISTANT_SATELLITE_VERSION
ENV PIP_BREAK_SYSTEM_PACKAGES=1

RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libasound2-plugins \
        alsa-utils \
        ffmpeg \
        python3 \
        python3-pip \
        python3-dev \
        git \
        alsa-utils \
        dkms \
        i2c-tools \
        libasound2-plugins \
    \
    && pip3 install --no-cache-dir -U \
        setuptools \
        wheel \
    && pip3 install --no-cache-dir \
        "homeassistant_satellite[silerovad,webrtc]@https://github.com/synesthesiam/homeassistant-satellite/archive/refs/tags/${HOMEASSISTANT_SATELLITE_VERSION}.tar.gz" \
    \
    && git clone "https://github.com/respeaker/seeed-voicecard" \
    && cd seeed-voicecard \
    && chmod a+x ./install.sh \
    && rm -rf .git \
    && apt-get remove --yes build-essential python3-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
    


# Copy script to run
COPY docker/run.sh /run.sh
RUN chmod a+x /run.sh

ENV CONFIG_PATH /data/options.json

ENTRYPOINT ["/run.sh"]
COPY sounds/ ./sounds/

WORKDIR /
COPY rootfs /
