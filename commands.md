# Usefull commands
## Move to directory
To execute all the commands user needs to navigate to 
```bash
    cd /home/mario/learning/confluent-kafka/cp-docker-images/examples/cp-all-in-one
```
## List all the topics
```bash
    docker-compose exec broker \
    kafka-topics --zookeeper zookeeper:2181 --list
```

docker-compose exec broker kafka-topics --zookeeper zookeeper:2181 --list
# List all the topics
