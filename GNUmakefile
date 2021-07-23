.PHONY: kill-all up alias

zookeeper := "127.0.0.01:2181"
bootstrap-server := "127.0.0.01:9091"

alias:
	sudo ifconfig lo0 alias 127.0.0.01;
	sudo ifconfig lo0 alias 127.0.0.02;
	sudo ifconfig lo0 alias 127.0.0.03;

up: kill-all alias
	KAFKA_CLUSTER=0 docker-compose -p kafka_cluster_0 -f kafka-docker-compose.yml up

kill-all:
	-docker ps -qa --filter="name=kafka_cluster" | xargs --no-run-if-empty docker kill
	-docker ps -qa --filter="name=kafka_cluster" | xargs --no-run-if-empty docker rm
	#docker network ls -q | grep _default | xargs docker network rm


list-topic-plain:
	kafka-topics.sh --bootstrap-server $(bootstrap-server) --command-config=kafka-client-plain.properties --describe

create-topic-plain:
	echo topic-{11..13} | xargs -n1 kafka-topics.sh --bootstrap-server $(bootstrap-server) \
	--command-config=kafka-client-plain.properties --create --replication-factor 3 --partitions 3 --topic

install-kafka:
	mkdir -p $$HOME/bin
	curl --silent https://mirrors.ukfast.co.uk/sites/ftp.apache.org/kafka/2.7.1/kafka_2.13-2.7.1.tgz | tar xvz -C $$HOME/bin

consume:
	kafka-console-consumer.sh --topic topic-11 --from-beginning \
	--consumer.config kafka-client-plain.properties --bootstrap-server $(bootstrap-server) \
	--property print.key=false

produce:
	kafka-console-producer.sh --broker-list $(bootstrap-server) \
		--topic topic-11
		--producer.config kafka-client-plain.properties