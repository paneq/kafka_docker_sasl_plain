[![Build Status](https://travis-ci.org/simplesteph/kafka-stack-docker-compose.svg?branch=master)](https://travis-ci.org/simplesteph/kafka-stack-docker-compose)


# kafka-stack-docker-compose

Based on: https://github.com/simplesteph/kafka-stack-docker-compose

## Stack version

  - Zookeeper version: 3.4.9
  - Kafka version: 2.1.1 (Confluent 5.1.2)

# Requirements

## Docker

Please export your environment before starting the stack:
```
export DOCKER_HOST_IP=127.0.0.1
```
(that's the default value and you actually don't need to do a thing)

## Docker-Toolbox
If you are using Docker for Mac <= 1.11, or Docker Toolbox for Windows
(your docker machine IP is usually `192.168.99.100`)

Please export your environment before starting the stack:
```
export DOCKER_HOST_IP=192.168.99.100
```

## Single Zookeeper / Single Kafka

This configuration fits most development requirements.

 - Zookeeper will be available at `$DOCKER_HOST_IP:2181`
 - Kafka will be available at `$DOCKER_HOST_IP:9092`


Run with:
```
docker-compose -f test up
```

# Users 

* `kafka_server_jaas.conf`
* Remember about `;` after the last user, doh.
* Internal docker interface does not use authentication so `ANONYMOUS` user is a superuser additionally.

# Notes

* `KAFKA_*` env variables are translated into settings `/etc/kafka/kafka.properties` (which tutorials and documentation usually refer to as `server.properties`)

# ACLs

```
kafka-acls --authorizer kafka.security.auth.SimpleAclAuthorizer --authorizer-properties zookeeper.connect=172.24.0.2:2181 --list

kafka-acls --authorizer kafka.security.auth.SimpleAclAuthorizer --authorizer-properties zookeeper.connect=172.24.0.2:2181  --add --allow-principal User:alice --producer --topic g

kafka-acls --authorizer kafka.security.auth.SimpleAclAuthorizer --authorizer-properties zookeeper.connect=172.24.0.2:2181  --add --allow-principal User:bob --consumer --topic g --group="BobGroup"
```

* No idea why `localhost:2181` is not good enough and fails but with numerical IP it works...

# Terraform

* https://github.com/Mongey/terraform-provider-kafka

```
provider "kafka" {
  bootstrap_servers = ["localhost:9092"]
  tls_enabled = false
  sasl_username = "admin"
  sasl_password = "admin"
}
```
