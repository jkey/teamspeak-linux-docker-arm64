#Download base image
FROM arm64v8/debian:bookworm-slim

LABEL Maintainer="Kurt Seiler <kurt@seiler.onl>"
LABEL Description="Teamspeak 3 Server on arm64v8 Debian Bookworm"

# Update and install dependencies
RUN apt update && apt install -y \
    wget \
    bzip2 \
    ca-certificates \
    tar \
    gpg

RUN set -eux; \
 addgroup --gid 9987 ts3server; \
 adduser -u 9987 --no-create-home --home /var/ts3server --ingroup ts3server --shell /usr/sbin/nologin --disabled-password ts3server; \
 install -d -o ts3server -g ts3server -m 775 /var/ts3server /var/run/ts3server /opt/ts3server

ENV PATH="${PATH}:/opt/ts3server"

# install Box64 and dependencies
RUN wget https://ryanfortner.github.io/box64-debs/box64.list -O /etc/apt/sources.list.d/box64.list \
    && wget -qO- https://ryanfortner.github.io/box64-debs/KEY.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/box64-debs-archive-keyring.gpg \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        box64

ARG TEAMSPEAK_CHECKSUM=775a5731a9809801e4c8f9066cd9bc562a1b368553139c1249f2a0740d50041e
ARG TEAMSPEAK_URL=https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2

RUN wget "${TEAMSPEAK_URL}" -O server.tar.bz2; \
    echo "${TEAMSPEAK_CHECKSUM}  server.tar.bz2" | sha256sum -c -; \
    mkdir -p /opt/ts3server; \
    tar -xf server.tar.bz2 --strip-components=1 -C /opt/ts3server; \
    rm server.tar.bz2; \
    mkdir -p /usr/lib/box64; \
    mv /opt/ts3server/*.so /opt/ts3server/redist/* /usr/lib/box64/

ENV BOX64_LD_LIBRARY_PATH="/usr/lib/box64" 
    

# setup directory where user data is stored
VOLUME /var/ts3server/
WORKDIR /var/ts3server/

#  9987 default voice
# 10011 server query
# 30033 file transport
EXPOSE 9987/udp 10011 30033 

COPY entrypoint.sh /opt/ts3server
RUN chmod +x /opt/ts3server/entrypoint.sh

ENTRYPOINT [ "entrypoint.sh" ]
CMD [ "ts3server" ]