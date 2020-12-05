---
title: 'Backend structure of a financial management app'
date: 2018-05-11
permalink: /posts/2018/05/review-backend-structure/
tags:
  - backend
  - server application
  - Netty
---

The whole system is a typical cilent-server structure. The client end has both Android and IOS version.
This blog will briefly introduce the backend server structure.

<!--more-->

> ![](https://xiaoluo-whu.github.io/files/images/project_structure.jpg)

In general, the backend has three layers: 

* interface layer: accept client's requests and assign them to corresponding service modules.

* service layer: core component of backend structure, performing various algorithms and calculations.

* storage layer: connecting service layer's output to databases. Select, Insert, Update, Delete.

[Netty](https://netty.io/)
as the framework, [MySQL](https://www.mysql.com/) & [Redis](https://redis.io/) & [MongoDB](https://www.mongodb.com/) for storage, [ETCD](https://etcd.io/) for synchronization,
[RabbitMQ](https://www.rabbitmq.com/) for message queue, [Docker](https://www.docker.com/) for production deployment, [Hive](https://hive.com/), [Hadoop](https://hadoop.apache.org/), [HBase](https://hbase.apache.org/)
and [Spark](https://spark.apache.org/) for big data management.