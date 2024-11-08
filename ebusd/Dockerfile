ARG BUILD_FROM
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

RUN apt-get update && \
    apt-get install wget -y

ARG EBUSD_VERSION=24.1

ARG ARCH

RUN case ${ARCH} in \
 "amd64") \
    wget -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_amd64-bookworm.deb && \
    dpkg -i ebusd.deb  ;; \
 "aarch64") \
    wget -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_arm64-bookworm.deb && \
    dpkg -i ebusd.deb  ;; \
 "armv7") \
    wget -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_armv7-bookworm.deb && \
    dpkg -i ebusd.deb  ;; \
 "armhf") \
    wget -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_armhf-bookworm.deb && \
    dpkg -i ebusd.deb  ;; \
 "i386") \
    wget -O ebusd.deb https://github.com/john30/ebusd/releases/download/${EBUSD_VERSION}/ebusd-${EBUSD_VERSION}_386-bookworm.deb && \
    dpkg -i ebusd.deb  ;; \
 *) echo "Unsupported architecture"; exit 1 ;; \
 esac

RUN apt-get install -y libmosquitto-dev libssl-dev \
    && rm -rf /var/lib/apt/lists/* \
    && rm -f ebusd.deb


LABEL Description="ebusd"

COPY run.sh /
RUN chmod a+x /run.sh
CMD [ "/run.sh" ]

# Health check
HEALTHCHECK --interval=5m --timeout=3s --start-period=90s \
   CMD nc -z 127.0.0.1 8888 || exit 1
