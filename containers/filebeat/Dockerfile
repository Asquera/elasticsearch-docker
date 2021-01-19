FROM docker.elastic.co/beats/filebeat:7.8.1

RUN mkdir -p /usr/share/filebeat/logs

USER root
RUN yum install -y nc
RUN touch /var/log/filebeat.log
RUN chmod 0644 /var/log/filebeat.log
RUN chown filebeat:filebeat /var/log/filebeat.log
USER filebeat

COPY --chown=root:filebeat ./filebeat.yml /usr/share/filebeat/filebeat.yml
