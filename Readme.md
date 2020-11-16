# Docker Setup

This is Docker setup for Elasticsearch and the ELK stack, useful to demonstrate examples in a workshop.

The following Docker containers are available

* Elasticsearch instances (7.8.1), a three node cluster, accessible from host, most examples only require first instance
  * http://elasticsearch01:9200
  * http://elasticsearch02:9200
  * http://elasticsearch03:9200
* Filebeat
* Redis as logbuffer
* Logstash to ingest logs into ES
* [Kibana](localhost:5601)
* [Cerebro](localhost:9000)

For more details check `docker-compose.yml` configuration.


## Setup

This repository uses Docker to set up a local environment.

* install [Docker](https://docs.docker.com/get-docker/) for your OS if not already present
* alternatively run the examples in a VM or a local development environment

Most examples require only a single Elasticsearch instance, therefore it's sufficient to start one instance and Cerebro to check the cluster.

```bash
docker-compose up --build elasticsearch01 cerebro
```

To start all containers at once (not advised) run `docker-compose up`, wait until all Docker containers are built.

The following services can be accessed via web browser.

* [Cerebro](http://localhost:9000/#/overview?host=http:%2F%2Felasticsearch01:9200)
* [Kibana](http://localhost:5601) (when Docker container is built and started)


## References

* [Elasticsearch Docker Setup](https://www.elastic.co/guide/en/elasticsearch/reference/7.3/docker.html)
* [3 Node Elasticsearch Cluster with Docker](https://blog.ruanbekker.com/blog/2018/04/29/running-a-3-node-elasticsearch-cluster-with-docker-compose-on-your-laptop-for-testing/)


## Tips

To check if the Redis instance is reachable, for example from within the filebeat / app docker container:

```bash
# https://stackoverflow.com/questions/33243121/abuse-curl-to-communicate-with-redis
$ exec 3<>/dev/tcp/redis/6379 && echo -e "PING\r\n" >&3 && head -c 7 <&3
+ PONG
```
This returns `+PONG` as response if successful.

To check if the filebeat configuration is loaded successfully, run `filebeat` with configuration:

```bash
$ filebeat -c filebeat.yml -e -d "*"
```

To emit events in the app container, run the following command

```bash
# connect into the running app container
$ docker exec -ti docker_app_1 /bin/bash
$ for i in {1..1000}; do echo "Hello World\n" >> /var/log/filebeat.log; done
$ for i in {1..1000}; do echo "BLUBB HURRA\n" >> /var/log/filebeat.log; done
```

all events are read from the `filebeat.log` log file and are send to Redis.


## Resources / Links

* Docker Compose Documentation https://docs.docker.com/compose/compose-file/#compose-and-docker-compatibility-matrix
* Docker Containers for Logstash / Elasticsearch / Kibana: https://www.docker.elastic.co/
* Logstash Docker Configuration https://www.elastic.co/guide/en/logstash/current/docker-config.html
* Logstash Configuration: https://www.elastic.co/guide/en/logstash/current/logstash-settings-file.html
* Elasticsearch Configuration: https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html
* Kibana Settings: https://www.elastic.co/guide/en/kibana/current/settings.html
* Filebeat Logging Configuration: https://www.elastic.co/guide/en/beats/filebeat/current/configuration-logging.html
