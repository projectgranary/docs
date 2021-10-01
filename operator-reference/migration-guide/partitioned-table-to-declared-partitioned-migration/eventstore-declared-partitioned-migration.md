---
description: >-
  This page contains a step by step guide for the migration of Profile Store
  from non-partitioned to partitioned.
---

# Event Store migration to Declarative Partitioning

## Steps

Rename the existing `eventstore` table and create new declared partitioned eventstore table:

```sql
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

Select all distinct `event_type` from `eventstore_old` table and then create partitioned tables with each value of `event_type` from the distinct values:

```sql
SELECT DISTINCT event_type FROM eventstore_old;

CREATE TABLE eventstore_science PARTITION OF eventstore FOR VALUES IN ('science')
CREATE TABLE eventstore_biology PARTITION OF eventstore FOR VALUES IN ('biology')
CREATE TABLE eventstore_robotics PARTITION OF eventstore FOR VALUES IN ('robotics')
...
```

Create indexes on the parent table `eventstore`:

```sql
CREATE UNIQUE INDEX eventstore_pkey ON eventstore(correlation_id text_ops,event_id text_ops,event_type text_ops);
CREATE INDEX eventstore_created_idx ON profilestore(created int8_ops);
CREATE INDEX eventstore_event_type_idx ON eventstore USING BRIN (event_type text_minmax_ops);
```

Once the tables are created, we can try to insert records using:

```sql
INSERT INTO eventstore (correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl) SELECT
correlation_id,event_id,created,event_type,event_type_version,event_harvester,message,partition_id,partition_offset,ttl FROM eventstore_old;
```

Then we can select the records with:

```sql
SELECT * from eventstore where event_type = 'science';
SELECT * from eventstore where event_type = 'biology';
SELECT * from eventstore where event_type = 'robotics';
```

