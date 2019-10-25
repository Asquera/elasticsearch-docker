FROM docker.elastic.co/elasticsearch/elasticsearch:7.3.2

RUN yum -y install vim

RUN curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
RUN python get-pip.py

RUN pip install elasticsearch-curator==5.5.4

COPY ./config/ /config

COPY --chown=elasticsearch:elasticsearch elasticsearch.yml /usr/share/elasticsearch/config/
