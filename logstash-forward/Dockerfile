#
# MAINTAINER        Carson,C.J.Zeong <zcy@nicescale.com>
# DOCKER-VERSION    1.11.1
#
# Dockerizing logstash-forwarder: Dockerfile for building logstash-forwarder images
#
FROM       zcy/centos7
MAINTAINER Carson,C.J.Zeong <zcy@nicescale.com>

ENV FILEBEAT_VERSION 1.2.2
ENV LOG_DIR /data/logs

RUN yum -y install rsyslog && \
	wget -q https://download.elastic.co/beats/filebeat/filebeat-${FILEBEAT_VERSION}-x86_64.rpm && \
    yum -y localinstall filebeat-${FILEBEAT_VERSION}.x86_64.rpm && \
    mkdir -p /data/logs

COPY supervisor_rsyslog.conf /etc/supervisor/confd/rsyslog.conf
COPY supervisor_filebeat.conf /etc/supervisor/confd/filebeat.conf


VOLUME ["/data/logs"]

EXPOSE 514