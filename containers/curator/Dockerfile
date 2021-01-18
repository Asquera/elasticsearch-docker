FROM python:3.8-alpine

RUN apk --no-cache add curl

RUN pip install "boto3<1.16" "botocore<1.19"

RUN pip install elasticsearch-curator==5.8.1 &&\
    rm -rf /var/cache/apk/*

COPY ./config /config

WORKDIR /config
