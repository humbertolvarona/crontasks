FROM alpine:latest

ENV TIMEZONE=America/Recife
ENV RCLONE_VERSION=v1.65.2
ENV HTTRACK_VERSION=3.49.2

RUN apk update && apk add --no-cache \
    python3 \
    py3-pip \
    openrc \
    wget \
    curl \
    axel \
    aria2 \
    lftp \
    mc \
    openssh-client \
    rsync \
    torsocks \
    git \
    dash \
    zsh \
    bash \
    nano \
    gawk \
    grep \
    sed \
    tzdata \
    unzip \
    && cp /usr/share/zoneinfo/$TIMEZONE /etc/localtime \
    && echo $TIMEZONE > /etc/timezone

RUN if [ "$RCLONE_VERSION" = "current" ]; then \
      wget -q -O /tmp/rclone.zip https://downloads.rclone.org/rclone-current-linux-amd64.zip ; \
    else \
      wget -q -O /tmp/rclone.zip https://downloads.rclone.org/${RCLONE_VERSION}/rclone-${RCLONE_VERSION}-linux-amd64.zip ; \
    fi \
    && unzip /tmp/rclone.zip -d /tmp \
    && cp /tmp/rclone-*-linux-amd64/rclone /usr/bin/ \
    && chmod 755 /usr/bin/rclone \
    && rm -rf /tmp/*

RUN apk add --no-cache \
    build-base \
    zlib-dev \
    openssl-dev \
    && wget https://mirror.httrack.com/historical/httrack-${HTTRACK_VERSION}.tar.gz -O /tmp/httrack.tar.gz \
    && tar -xzf /tmp/httrack.tar.gz -C /tmp \
    && cd /tmp/httrack-3.49.2 \
    && ./configure && make && make install \
    && cd / \
    && rm -rf /tmp/* \
    && apk del build-base zlib-dev openssl-dev

RUN mkdir -p /usr/src/app/shared/data /usr/src/app/crontasks /usr/src/app/scripts \
    && touch /var/log/cron.log

RUN sed -i 's|/bin/ash|/bin/bash|' /etc/passwd

CMD ["sh", "-c", "chmod +x /usr/src/app/scripts/* && cp /usr/src/app/crontasks/crontab /etc/crontabs/root && chmod 0644 /etc/crontabs/root && crond -f && tail -f /var/log/cron.log"]
