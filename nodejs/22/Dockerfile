FROM --platform=$TARGETOS/$TARGETARCH node:22-bookworm-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates \
    dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
 && rm -rf /var/lib/apt/lists/* \
 && useradd -m -d /home/container container

RUN npm config set fetch-retries 5 \
 && npm config set fetch-retry-mintimeout 20000 \
 && npm config set fetch-retry-maxtimeout 120000

RUN npm i -g --no-audit --no-fund typescript ts-node @types/node

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
