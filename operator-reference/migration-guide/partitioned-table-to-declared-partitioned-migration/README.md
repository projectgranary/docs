---
description: This page is for the introduction of partitioned and its use.
---

# Declarative Partitioning Migration guides

## Introduction

Partitioning refers to splitting what is logically one large table into smaller physical pieces.

In PostgreSQL, the partitioned table itself is a “virtual” table having no storage of its own. Instead, the storage belongs to _partitions_, which are otherwise-ordinary tables associated with the partitioned table. Each partition stores a subset of the data as defined by its _partition bounds_. All rows inserted into a partitioned table will be routed to the appropriate one of the partitions based on the values of the partition key column(s). Updating the partition key of a row will cause it to be moved into a different partition if it no longer satisfies the partition bounds of its original partition. 

We are going to use ["List" Partitioning](https://www.postgresql.org/docs/12/ddl-partitioning.html): the table is partitioned by explicitly listing which key value(s) appear in each partition.

{% hint style="warning" %}
To avoid corrupt data or even data lost during the migration to declarative partitions, the safest method for **Profile Store** migration is to accept the downtime and to stop the profile updater(s). Check the topic retention of profile updater to determine the maximum length of your downtime window.\
\
Same applies for the migration of **Event Store**. The safest method is to stop the Persisters that write to the database. Here it is possible to migrate the data event type by event type. I.e. only one persister at a time needs to have a downtime.\
\
Generally speaking, the downtime depends on the size of the data in the database and the database server's resources.
{% endhint %}

## Motivation

In Granary, we promote the usage of declarative partitioning for the two main tables:

* [Event Store](../../../developer-reference/dataflow/event-store/#table-eventstore)
* [Profile Store](../../../developer-reference/dataflow/profile-store/#table-profilestore)

Measurements have shown that by using declarative partitioning, performance gains of factor 2-4 are possible depending on the data volume. The larger the queried data set was, the bigger the observed performance gain. Insertion to the tables remains on equally performant level as before.
