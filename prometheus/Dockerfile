FROM alpine:latest

RUN apk --no-cache add --virtual build-dependencies wget \
    && apk --no-cache add ca-certificates \
    && mkdir -p /tmp/install /tmp/dist \
    && wget -O /tmp/install/prometheus.tar.gz https://github.com/prometheus/prometheus/releases/download/v$VERSION/prometheus-$VERSION.linux-$ARCH.tar.gz \
    && apk del build-dependencies \
    && cd /tmp/install \
    && tar --strip-components=1 -xzf prometheus.tar.gz \
    && mkdir -p /etc/prometheus /usr/share/prometheus \
    && mv prometheus promtool   /bin/ \
    && mv prometheus.yml        /etc/prometheus/prometheus.yml \
    && mv consoles console_libraries NOTICE LICENSE /usr/share/prometheus/ \
    && ln -s /usr/share/prometheus/console_libraries /usr/share/prometheus/consoles/ /etc/prometheus/ \
    && rm -rf /tmp/install

RUN adduser -H -D -u 1001 openshift && usermod -aG 0 openshift && \
chown -R 1001 /opt && \
chgrp -R 0 /opt  && \
chmod -R g=u /opt

USER 1001

EXPOSE 9090

CMD /opt/prometheus/prometheus --config.file=/opt/prometheus-config/prometheus.yml --storage.tsdb.path=/opt/prometheus-data
