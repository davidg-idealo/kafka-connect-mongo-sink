# kafka-connect-mongo-sink
Playground project to understand the basics of Kafka connect.

Kafka UI: http://localhost:8000/

Kafka Connect UI: http://localhost:8003/#/cluster/kafka-connect-1

For the producer
Download kafka-tools: https://www.apache.org/dyn/closer.cgi?path=/kafka/2.3.0/kafka_2.12-2.3.0.tgz

Create a topic
https://kafka.apache.org/quickstart#quickstart_download

./kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic todos-v1



