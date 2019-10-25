FROM docker.elastic.co/beats/filebeat:7.3.2

RUN mkdir -p /usr/share/filebeat/logs

USER root
RUN touch /var/log/filebeat.log
RUN chmod 0644 /var/log/filebeat.log
RUN chown filebeat:filebeat /var/log/filebeat.log
USER filebeat

COPY --chown=root:filebeat ./filebeat.yml /usr/share/filebeat/filebeat.yml
