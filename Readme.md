docker-hadoop-cluster Build Status

Multiple node cluster on Docker for self development. docker-hadoop-cluster is suitable for testing your patch for Hadoop which has multiple nodes.
Build images from your Hadoop source code

Base image of hadoop service. This image includes JDK, hadoop package configurations etc. This image can include your self-build hadoop package. In order to bind, tar.gz package assumed be put under hadoop-base directory.

$ cd docker-hadoop-cluster
$ cp /path/to/hadoop-3.0.0-alpha3-SNAPSHOT.tar.gz hadoop-base
$ make

Once you build hadoop-base image, you can launch hadoop cluster by using docker-compose.

$ docker-compose up -d

or

$ make run

See http://localhost:9870 for NameNode or http://localhost:8088 for ResourceManager.

$ make down

shutdowns your cluster.
Build images from the latest trunk

docker-hadoop-cluster also uploads the latest image which refers HEAD of trunk. They are deployed on Docker Hub. If you want to try the trunk (though it can be unstable), docker-compose.yml like below is needed. It will launch 3 slave Hadoop cluster.

version: '2'

services:
  master:
    image: lewuathe/hadoop-master
    ports:
      - "9870:9870"
      - "8088:8088"
      - "19888:19888"
      - "8188:8188"
    container_name: "master"
  slave1:
    image: asma24/hadoop-slave
    container_name: "slave1"
    depends_on:
      - master
    ports:
      - "9901:9864"
      - "8041:8042"
  slave2:
    image: asma24/hadoop-slave
    container_name: "slave2"
    depends_on:
      - master
    ports:
      - "9902:9864"
      - "8042:8042"
 

Login cluster

$ docker exec -it master bash
bash-4.1# cd /usr/local/hadoop
bash-4.1# bin/hadoop version
Hadoop 3.0.0-SNAPSHOT
Source code repository git://git.apache.org/hadoop.git -r 0c7d3f480548745e9e9ccad1d318371c020c3003
Compiled by lewuathe on 2015-09-13T01:12Z
Compiled with protoc 2.5.0
From source with checksum 9174a352ac823cdfa576f525665e99
