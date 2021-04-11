# Run elasticsearch
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 -d -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.12.0

# Run Kibana
docker run --link elasticsearch:elasticsearch -d -p 5601:5601 docker.elastic.co/kibana/kibana:7.12.0

# Verify
curl -v "localhost:9200/_cat/nodes?v=true&pretty"

# Install Fluentd

Kubectl apply -f fluentd/

# Install Custom Fluentd
docker run -d -p 24224:24224 -p 5140:5140 --name custom-docker-fluent-logger -v $(pwd)/log:/fluentd/log custom-fluentd:latest

# Verify 
docker run --rm --log-driver=fluentd ubuntu /bin/echo 'JG Hello world'

---
docker run -it -p 24224:24224 -v $(pwd)/custom-fluentd/conf/test.conf:/fluentd/etc/test.conf -e FLUENTD_CONF=test.conf fluent/fluentd:latest

docker run --log-driver=fluentd -it ubuntu:latest bash

The fluentd logging driver sends container logs to the Fluentd collector as structured log data. Then, users can use any of the various output plugins of Fluentd to write these logs to various destinations.

In addition to the log message itself, the fluentd log driver sends the following metadata in the structured log message:

**container_id**	The full 64-character container ID.
**container_name**	The container name at the time it was started. If you use docker rename to rename a container, the new name is not reflected in the journal entries.
**source**	stdout or stderr
**log**	The container log

**Example container log of Fluentd**:
```
2021-04-11 09:14:26.000000000 +0000 225ea84275fd: 
{
	"container_id":"225ea84275fd99a05066c00a7f96ca6d079e355315b5013ca16123525252c1f2",
	"container_name":"/practical_hoover",
	"source":"stdout",
	"log":"Hello\r"
} 
```
