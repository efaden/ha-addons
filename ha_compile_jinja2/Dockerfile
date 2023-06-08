# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:6.2.5
FROM $BUILD_FROM


# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy Python requirements file
COPY requirements.txt /tmp/requirements.txt

RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        python3-dev=3.9.2-3 \
        python3=3.9.2-3 \
        python3-distutils \
        nodejs \
        npm \
        inotify-tools \
    \
    && npm install -g prettier \
    \
    && curl https://bootstrap.pypa.io/get-pip.py | python3 \
    \
    && update-alternatives \
        --install /usr/bin/python python /usr/bin/python3 10 \
    \
    && pip3 install --no-cache-dir -r /tmp/requirements.txt \
    \
    && apt-get purge -y --auto-remove \
        bsdtar \
        python3-dev \
    \
    && find /usr/local \
        \( -type d -a -name test -o -name tests -o -name '__pycache__' \) \
        -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
        -exec rm -rf '{}' + \
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
    chmod a+x /usr/bin/ha_compile_jinja

# CMD /usr/bin/ha_compile_jinja