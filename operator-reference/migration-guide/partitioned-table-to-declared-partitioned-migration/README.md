# Partitioned table to Declared partitioned migration

## Introduction

Partitioning refers to splitting what is logically one large table into smaller physical pieces.

The partitioned table itself is a “virtual” table having no storage of its own. Instead, the storage belongs to _partitions_, which are otherwise-ordinary tables associated with the partitioned table. Each partition stores a subset of the data as defined by its _partition bounds_. All rows inserted into a partitioned table will be routed to the appropriate one of the partitions based on the values of the partition key column\(s\). Updating the partition key of a row will cause it to be moved into a different partition if it no longer satisfies the partition bounds of its original partition. 

We are going to use `List Partitioning` - the table is partitioned by explicitly listing which key value\(s\) appear in each partition.

