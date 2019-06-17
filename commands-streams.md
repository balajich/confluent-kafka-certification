# Kafka Streams 
Contains useful commands for kafka streams
# Commands
## Move to directory
To execute all the commands user needs to navigate to 
```bash
    cd /home/mario/learning/confluent-kafka/cp-docker-images/examples/cp-all-in-one
```
## List all the topics
```bash
    docker-compose exec broker \
    kafka-topics --zookeeper zookeeper:2181 \
                 --list
```
## Create topics for first stream application
### Create a plaintext input topic
```bash
   docker-compose exec broker \
   kafka-topics --zookeeper zookeeper:2181 \
                --create --replication-factor 1 --partitions 1 --topic streams-plaintext-input
```
### Create a plaintext output topic
```bash
   docker-compose exec broker \
   kafka-topics --zookeeper zookeeper:2181 \
                --create --replication-factor 1 --partitions 1 --topic streams-wordcount-output
```
### Produce data to input topic
Produce a raw data to input topic  using kafka-console-producer application. The streams application is going to read from raw topic
```bash
   docker-compose exec broker \
   kafka-console-producer --broker-list broker:9092 \
                --topic streams-plaintext-input
```
### Verify whether the data is properly produced to input topic or not
Using kaka-console-consumer application subscribe to input topic and observer whether the data is published properly or not.
```bash
   docker-compose exec broker \
   kafka-console-consumer --bootstrap-server broker:9092 \
                --topic streams-plaintext-input \
                --from-beginning
```
### Start Stream application
Word count  stream application is pre bundled in kafka. Lets run it on input topic
```bash
   docker-compose exec broker \
   kafka-run-class org.apache.kafka.streams.examples.wordcount.WordCountDemo
```

### Start Kafka consumer on output topic
```bash
   docker-compose exec broker \
   kafka-console-consumer --bootstrap-server localhost:9092 \
    --topic streams-wordcount-output \
    --from-beginning \
    --formatter kafka.tools.DefaultMessageFormatter \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```
