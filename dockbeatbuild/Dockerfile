FROM alpine:latest

# Needa change these versions if wanted
ENV GLIBC_APK_VERSION=2.23-r3 \
    DOCKBEAT_VERSION=1.0.0

ADD https://github.com/Ingensi/dockbeat/releases/download/v${DOCKBEAT_VERSION}/dockbeat-v${DOCKBEAT_VERSION}-x86_64 /usr/local/bin/dockbeat

RUN chmod +x /usr/local/bin/dockbeat && \
    # The downloaded Dockbeat binary is static compiled against glibc (not musl); so it needs
    # a functional copy of glibc, which is available from https://github.com/sgerrand/alpine-pkg-glibc
    apk --no-cache add ca-certificates openssl && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub && \
    wget -q https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_APK_VERSION}/glibc-${GLIBC_APK_VERSION}.apk && \
    apk add glibc-${GLIBC_APK_VERSION}.apk && \
    rm glibc-${GLIBC_APK_VERSION}.apk

WORKDIR /etc/dockbeat

CMD [ "/usr/local/bin/dockbeat", "-c", "/etc/dockbeat/dockbeat.yml", "-e" ]
