FROM python:3.7-alpine

RUN apk --no-cache add curl
RUN pip install elasticsearch-curator==5.7.6 &&\
    rm -rf /var/cache/apk/*

COPY ./config/ /config

RUN /usr/bin/crontab /config/crontab.txt

CMD ["/usr/sbin/crond","-f"]
