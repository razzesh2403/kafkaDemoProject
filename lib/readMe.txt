#Steps to follow once the zookeepr and kafka broker are up and running
1. create the input topic named streams-plaintext-input and the output topic named streams-wordcount-output:

> kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic streams-plaintext-input 

#output : Created topic "streams-plaintext-input".

2. create the output topic with compaction enabled because the output stream is a changelog stream (cf. explanation of application output below).

> kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic streams-wordcount-output --config cleanup.policy=compact

#output : Created topic "streams-wordcount-output".

3. created topic can be described with the kafka-topics tool:

> kafka-topics.bat --zookeeper localhost:2181 --describe
 
#Topic:streams-plaintext-input   PartitionCount:1    ReplicationFactor:1 Configs:
#    Topic: streams-plaintext-input  Partition: 0    Leader: 0   Replicas: 0 Isr: 0
#Topic:streams-wordcount-output  PartitionCount:1    ReplicationFactor:1 Configs:cleanup.policy=compact
#    Topic: streams-wordcount-output Partition: 0    Leader: 0   Replicas: 0 Isr: 0

4. Start the Wordcount Application (before you run the Java Program, check weather all the required jars in /lib folder added to build path or not.)

5. Start a producer on the input topic streams-plaintext-input
>  kafka-console-producer.bat --broker-list localhost:9092 --topic streams-plaintext-input

6. Start a consumer on the output topic 
> kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic streams-wordcount-output --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property print.value=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer

7. start playing
	publish some data on the publisher and observe the word count on the consumer.
