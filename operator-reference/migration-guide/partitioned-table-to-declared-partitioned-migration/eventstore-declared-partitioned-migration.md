---
description: >-
  This page contains a step by step guide for the migration of Profile Store
  from non-partitioned to partitioned.
---

# Event Store migration to Declarative Partitioning

## Steps

1\) Create new EventStore table `eventstore_new`with declared partitioned by List `event_type` : 

```sql
CREATE TABLE eventstore_new (
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

2\) Select all distinct `event_type` from `eventstore_old` table and then create partitioned tables with each value of `event_type` from the distinct values:

```sql
SELECT DISTINCT event_type FROM eventstore;

CREATE TABLE eventstore_science PARTITION OF eventstore_new FOR VALUES IN ('science')
CREATE TABLE eventstore_biology PARTITION OF eventstore_new FOR VALUES IN ('biology')
CREATE TABLE eventstore_robotics PARTITION OF eventstore_new FOR VALUES IN ('robotics')
...
```

3\) Create indexes on the parent table `eventstore`:

```sql
CREATE UNIQUE INDEX eventstore_pkey ON eventstore_new(correlation_id text_ops,event_id text_ops,event_type text_ops);
CREATE INDEX eventstore_created_idx ON eventstore_new(created int8_ops);
CREATE INDEX eventstore_event_type_idx ON eventstore_new USING BRIN (event_type text_minmax_ops);
```

4\) Once the tables are created, we can try to insert records for one by one `event_type`. Be sure to stop the corresponding Event Store persister and then run the following command:

```sql
INSERT INTO eventstore_new (correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl) SELECT
correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl FROM eventstore where event_type = 'science';

INSERT INTO eventstore_new (correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl) SELECT
correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl FROM eventstore where event_type = 'biology';

...
```

5\) Then we can select the records with:

```sql
SELECT * from eventstore_new where event_type = 'science';
SELECT * from eventstore_new where event_type = 'biology';
SELECT * from eventstore_new where event_type = 'robotics';
```

6\) Rename the table `eventstore` to `eventstore_original` and then rename the `eventstore_new` to`eventstore` table. Before doing so, ensure that there are not open connections to the original table.

```sql
ALTER TABLE eventstore rename to eventstore_original;
ALTER TABLE eventstore_new rename to eventstore;
```

