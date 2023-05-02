# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:6.2.5
FROM $BUILD_FROM


# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        inotify-tools \
    \
    && rm -fr \
        /root/.cache \
        /tmp/* \
        /var/{cache,log}/* \
        /var/lib/apt/lists/*   

# Copy root filesystem
COPY rootfs /

# Make Runnable
RUN \
    chmod a+x /usr/bin/ha_dashboard_toucher
