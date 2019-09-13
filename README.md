# kafka-connect-mongo-sink
Playground project to understand the basics of Kafka connect.

Kafka UI: http://localhost:8000/

Kafka Connect UI: http://localhost:8003/#/cluster/kafka-connect-1

For the producer
Download kafka-tools: https://www.apache.org/dyn/closer.cgi?path=/kafka/2.3.0/kafka_2.12-2.3.0.tgz

Create a topic
https://kafka.apache.org/quickstart#quickstart_download

./kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic todos-v1

curl -X POST \
  http://localhost:8083/connectors \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "name": "MongoSinkConnector",
  "config": {
    "connector.class": "com.mongodb.kafka.connect.MongoSinkConnector",
    "topics": "todos-v1",
    "tasks.max": "1",
    "database": "repo",
    "collection": "todos",
    "connection.uri": "mongodb://localhost:27017"
     "key.converter": "org.apache.kafka.connect.json.JsonConverter",
     "key.converter.schemas.enable": "false",
     "value.converter": "org.apache.kafka.connect.json.JsonConverter",
     "value.converter.schemas.enable": "false"
  }
}'

curl -X PUT http://localhost:8083/connectors/MongoSinkConnector/config \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
    "connector.class": "com.mongodb.kafka.connect.MongoSinkConnector",
    "topics": "todos-v1",
    "tasks.max": "1",
    "database": "repo",
    "collection": "todos",
    "connection.uri": "mongodb://localhost:27017",
     "key.converter": "org.apache.kafka.connect.storage.StringConverter‌",
     "key.converter.schemas.enable": "false",
     "value.converter": "org.apache.kafka.connect.storage.StringConverter‌",
     "value.converter.schemas.enable": "false"
}'

curl -X PUT http://localhost:8083/connectors/MongoSinkConnector/config \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
    "connector.class": "com.mongodb.kafka.connect.MongoSinkConnector",
    "topics": "todos-v3",
    "tasks.max": "1",
    "database": "repo",
    "collection": "todos",
    "connection.uri": "mongodb://mongodb:27017",
     "key.converter": "org.apache.kafka.connect.storage.StringConverter‌",
     "key.converter.schemas.enable": "false",
     "value.converter": "org.apache.kafka.connect.storage.StringConverter‌",
     "value.converter.schemas.enable": "false"
}'

