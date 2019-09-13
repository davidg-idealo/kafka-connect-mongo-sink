# kafka-connect-mongo-sink
Playground project to understand the basics of Kafka connect.
# Goal
Write data into a Kafka topic and let Kafka Connect consume this topic and persist these entries into a MongoDB.

## Setup the environment
We use Docker to ramp up the complete playground environment. So, install Docker on your machine.

Ensure that you provide Docker at least 8 GB of RAM. This hint is from the [Confluent-Quickstart](https://docs.confluent.io/current/quickstart/ce-docker-quickstart.html#download-start-cp-docker).

To download all containers and start it just use:

```
docker-compose up -d
```

The docker-compose file is heavily inspired by: https://github.com/confluentinc/examples/blob/5.3.1-post/cp-all-in-one/docker-compose.yml

We just added Kafka-UI, Kafka-Connect-UI, Mongo and remove all KSQL-components and the Control Center.

After the initialization you should get an overview of all running containers with

```
docker-compose ps
```

```
broker             /etc/confluent/docker/run     Up      0.0.0.0:29092->29092/tcp, 0.0.0.0:9092->9092/tcp
connect            /etc/confluent/docker/run     Up      0.0.0.0:8083->8083/tcp, 9092/tcp
kafka-connect-ui   /run.sh                       Up      0.0.0.0:8003->8000/tcp
kafka-topics-ui    /run.sh                       Up      0.0.0.0:8000->8000/tcp
mongodb            docker-entrypoint.sh mongod   Up      0.0.0.0:27017->27017/tcp
rest-proxy         /etc/confluent/docker/run     Up      0.0.0.0:8082->8082/tcp
schema-registry    /etc/confluent/docker/run     Up      0.0.0.0:8081->8081/tcp
zookeeper          /etc/confluent/docker/run     Up      0.0.0.0:2181->2181/tcp, 2888/tcp, 3888/tcp
```

To check if everything works well, you can open the two UI applications:

Kafka UI: http://localhost:8000/

Kafka Connect UI: http://localhost:8003/#/cluster/kafka-connect-1

It takes a minute until everthing is wired and ready for take off. So don't be nervous if you error messages in the UI on the first call.

## Create a kafka topic
At first you need the kafka-tools, which is a collection of [shell scripts](https://github.com/apache/kafka/tree/trunk/bin)

You can download the whole package at https://www.apache.org/dyn/closer.cgi?path=/kafka/2.3.0/kafka_2.12-2.3.0.tgz

Extract the package and fire in the bin folder the following command:
```
./kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic users-v1
```
The result will be a new topic with the name users-v1, which should be displayed in the Kafka-UI

For more information, just check the quickstart from the Kafka documentation: https://kafka.apache.org/quickstart#quickstart_download

## Publish kafka messages with the console producer
```
for (( i=0; i<100; i++)); do echo writing record ..$i; ./kafka-console-producer.sh --broker-list localhost:9092 --topic todos-v3 < user.json; done
```

## Configure Mongo Sink in Kafka Connect
TODO: 
* Source of the mongo-kafka sink
* How to setup the sink
https://github.com/mongodb/mongo-kafka/blob/master/docs/sink.md
```
curl -X POST \
  http://localhost:8083/connectors \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "name": "MongoSinkConnector",
  "config": {
    "connector.class": "com.mongodb.kafka.connect.MongoSinkConnector",
    "topics": "users-v1",
    "tasks.max": "1",
    "database": "repo",
    "collection": "users",
    "connection.uri": "mongodb://mongodb:27017"
     "key.converter": "org.apache.kafka.connect.storage.StringConverter",
     "key.converter.schemas.enable": "false",
     "value.converter": "org.apache.kafka.connect.storage.StringConverter",
     "value.converter.schemas.enable": "false"
  }
}'
```


