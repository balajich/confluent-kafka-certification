# Useful commands
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
## Create topic
```bash
   docker-compose exec broker \
   kafka-topics --zookeeper zookeeper:2181 \
                --create --topic my-first-topic  --replication-factor 1 --partitions 1
```
docker-compose exec broker kafka-topics --zookeeper zookeeper:2181 --list
# List all the topics
