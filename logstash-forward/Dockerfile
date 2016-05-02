#
# MAINTAINER        Carson,C.J.Zeong <zcy@nicescale.com>
# DOCKER-VERSION    1.11.1
#
# Dockerizing logstash-forwarder: Dockerfile for building logstash-forwarder images
#
FROM       zcy/centos7
MAINTAINER Carson,C.J.Zeong <zcy@nicescale.com>

ENV LOGSTASH_FORWARDER_VERSION 0.4.0-1
ENV LOG_DIR /data/logs

RUN yum -y install rsyslog && \
    wget -q https://download.elastic.co/logstash-forwarder/binaries/logstash-forwarder-${LOGSTASH_FORWARDER_VERSION}.x86_64.rpm && \
    yum -y localinstall logstash-forwarder-${LOGSTASH_FORWARDER_VERSION}.x86_64.rpm && \
    mkdir -p /data/logs

COPY logstash-forwarder.conf /etc/logstash-forwarder.conf
COPY logstash-forwarder.crt /etc/pki/tls/certs/logstash-forwarder.crt
COPY logstash-beats.crt /etc/pki/tls/certs/logstash-beats.crt
COPY supervisor_rsyslog.conf /etc/supervisor/confd/rsyslog.conf
COPY supervisor_logstash-forwarder.conf /etc/supervisor/confd/logstash-forwarder.conf
COPY filebeat.yml /etc/filebeat/filebeat.yml

VOLUME ["/data/logs"]

EXPOSE 514