# Run elasticsearch
docker run --name quizzical_galileo -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.12.0

# Run Kibana
docker run --link quizzical_galileo:elasticsearch -p 5601:5601 docker.elastic.co/kibana/kibana:7.12.0

# Verify
curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"

# Install Fluentd

$Kubectl apply -f fluentd/
