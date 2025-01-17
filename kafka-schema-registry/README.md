# Kafka schema-registry

## How to connect and run schema registry commands?

### docker-commands
docker exec -it schema-registry /bin/bash

### useful kafka commands

kafka-avro-console-producer \
  --broker-list broker:29092 --topic bar \
  --property value.schema='{"type":"record","name":"myrecord","fields":[{"name":"f1","type":"string"}]}'

 sample mesages
 {"f1": "value1"}
 {"f2": "value2"}

 kafka-avro-console-consumer \
 --topic customer-avro \
 --bootstrap-server broker:29092 \
 --from-beginning

**Where does kafka stores schema's?**

   Kafka stores schema infromation in _schema topic.

**What are the schema registry properties we need to provide in producer to register schema?**

    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
    props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,io.confluent.kafka.serializers.KafkaAvroSerializer.class);
    props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, io.confluent.kafka.serializers.KafkaAvroSerializer.class);
    props.put("schema.registry.url", "http://localhost:8081");

**How to registry schema for a key?**

    props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,io.confluent.kafka.serializers.KafkaAvroSerializer.class);
    This will register schema for a key.

**What is a specificrecord or genericRecord?**

    Specificrecord:
        It is a auto generated pojo class by maven plugin when a .avsc schema file is provided.
        This will throw an compilation error when a field is missing.
    GenericRecord:
        schema will be provided explicitly and field will be accessed either by name or index.
        This will throw an compilation error when a field is missing.

**Will kafka producer always register schema for each produced message?**

    No. schema will be registered when the first message is published and it will be cached in a hashmap by KafkaAvroSerializer.
    Next time it will retrive it from cache.

**Will kafka consumer always fetch schema for each consumed message?**

    No. schema will be registered when the first message is fetched from registry and it will be cached in a hashmap by KafkaAvroDeSerializer.
    Next time it will retrive it from cache.

**When deserialize a message if schema is not avilable in cache**

   Schema will be fetched from schema registry.

**What is magicbyte?**

    magicbyte is a schema id got after schema is registered with schema registry.
    It will appened to the message and consumer will refer to this while decoding the message.

**What are type of compatibility schema registry supports and how?**

    FORWARD
    BACKWARD
    FULL

**What is fully compatable schema?**

**What is Forward compatable schema?**

**What is Backward compatable schema?**

**will a custom pojo can be auto registered in register schema?**

    No, this has to be handled explicitly and you need to modify your class similar to auto generate class by maven plugin.
