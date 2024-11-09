ARG BUILD_FROM
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

RUN apt-get update && \
    apt-get install wget -y


ARG BUILD_ARCH
ENV ARCH=${BUILD_ARCH}
RUN echo "Building for architecture: ${ARCH}"

ARG EBUSD_VERSION=24.1


RUN \
    if [ "${BUILD_ARCH}" = "amd64" ]; then \
        wget -q -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_amd64-bookworm.deb && \
        dpkg -i ebusd.deb  ; \
    elif [ "${BUILD_ARCH}" = "aarch64" ]; then \
        wget -q -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_arm64-bookworm.deb && \
        dpkg -i ebusd.deb  ; \
    elif [ "${BUILD_ARCH}" = "armhf" ] || [ "${BUILD_ARCH}" = "armv7" ]; then \
        wget -q -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_armv7-bookworm.deb && \
        dpkg -i ebusd.deb  ; \
    elif [ "${BUILD_ARCH}" = "i386" ]; then \
        wget -q -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_386-bookworm.deb && \
        dpkg -i ebusd.deb  ; \
#    elif [ "${BUILD_ARCH}" = "armhf" ]; then \
#        wget -q -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_armv6l-buster.deb        && \
#        dpkg -i ebusd.deb  ; \
    else \
        echo "Unsupported architecture"; exit 1; \
    fi

RUN apt-get install -y libmosquitto-dev libssl-dev \
    && rm -rf /var/lib/apt/lists/* \
    && rm -f ebusd.deb


LABEL Description="ebusd"

COPY run.sh /
RUN chmod a+x /run.sh
CMD [ "/run.sh" ]

# Health check
HEALTHCHECK --interval=1m --timeout=3s --start-period=120s \
   CMD nc -z 127.0.0.1 8888 || exit 1
