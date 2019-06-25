# kafka

./opt/kafka_2.11-0.10.1.0/bin/kafka-console-consumer.sh
./opt/kafka_2.11-0.10.1.0/bin/kafka-console-producer.sh



docker exec -it  kafka_and_zk kafka-topics.sh --zookeeper kafka_and_zk:2181 --list

kafka-topics.sh --zookeeper kafka_and_zk:2181 --list

docker exec -it daas_kafka kafka-topics.sh --zookeeper daas_zookeeper:2181 --list

docker exec -it daas_kafka kafka-topics.sh --create --zookeeper daas_zookeeper:2181 --replication-factor 1 --partitions 1 --topic test

docker exec -it daas_kafka kafka-console-producer.sh --broker-list localhost:9092 --topic events

docker exec -it daas_kafka kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic events --from-beginning







需要在kafka的bin目录下

查看所有的topic

./kafka-topics.sh --zookeeper kafka_and_zk:2181 --list

查看某一topic的详细信息

./kafka-topics.sh --zookeeper kafka_and_zk:2181 --topic daas-push-event --describe





















ai@aiai:/opt/consul$ ./consul agent -dev -ui