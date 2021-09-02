---
description: >-
  This page explained the step by step guild of migration of EventStore
  Non-partitioned to Declared-partitioned.
---

# EventStore Declared-partitioned migration

## Introduction

Partitioning refers to splitting what is logically one large table into smaller physical pieces.

The partitioned table itself is a “virtual” table having no storage of its own. Instead, the storage belongs to _partitions_, which are otherwise-ordinary tables associated with the partitioned table. Each partition stores a subset of the data as defined by its _partition bounds_. All rows inserted into a partitioned table will be routed to the appropriate one of the partitions based on the values of the partition key column\(s\). Updating the partition key of a row will cause it to be moved into a different partition if it no longer satisfies the partition bounds of its original partition. 

We are going to use `List Partitioning` - the table is partitioned by explicitly listing which key value\(s\) appear in each partition.

## Steps

Rename the existing eventstore table and create new declared partitioned eventstore table -

```text
ALTER TABLE eventstore rename to eventstore_old;

CREATE TABLE eventstore (
    correlation_id character varying,
    event_id character varying,
    created bigint NOT NULL,
    event_type character varying,
    event_type_version bigint NOT NULL DEFAULT 1,
    event_harvester character varying NOT NULL,
    message jsonb,
    partition_id bigint NOT NULL,
    partition_offset bigint NOT NULL,
    ttl character varying,
    CONSTRAINT eventstore_pkey PRIMARY KEY (correlation_id, event_id, event_type)
)PARTITION BY LIST (event_type);
```

Create partitioned table with the each partitioned value event\_type -

```text
CREATE TABLE eventstore_science PARTITION OF eventstore FOR VALUES IN ('science')
CREATE TABLE eventstore_biology PARTITION OF eventstore FOR VALUES IN ('biology')
CREATE TABLE eventstore_robotics PARTITION OF eventstore FOR VALUES IN ('robotics')
```

Create indexes on the parent table eventstore -

```text
CREATE UNIQUE INDEX eventstore_pkey ON eventstore(correlation_id text_ops,event_id text_ops,event_type text_ops);
CREATE INDEX eventstore_created_idx ON profilestore(created int8_ops);
CREATE INDEX eventstore_event_type_idx ON eventstore USING BRIN (event_type text_minmax_ops);
```

Once the tables are created we can try to insert record using -

```text
insert into eventstore (correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl) values
('06ca9b36-8e65-49a9-8bcd-677de1554e53','00005c4a-4486-406f-9160-109e375b21cb',1413276032010,'science',4,'hvs-science-api','{"appid": "science-api", "payload": {"kundennummer": "32112"}}',17,435892,Null)

insert into eventstore (correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl) values
('324dw36-8e65-49a9-8bcd-677de1554e53','04555c4a-4486-406f-9160-109e375b21cb',1613276032010,'biology',22,'hvs-biology-api','{"appid": "biology-api", "payload": {"kundennummer": "789812"}}',21,3222,Null)

insert into eventstore (correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl) values
('d3a9b36-8e65-49a9-8bcd-677de1554e53','30005c4a-4486-406f-9160-109e375b21cb',1813276032010,'robotics',15,'hvs-robotics-api','{"appid": "robotics-api", "payload": {"kundennummer": "89322"}}',7,9892,Null)
```

Then we can select the records with -

```text

```

