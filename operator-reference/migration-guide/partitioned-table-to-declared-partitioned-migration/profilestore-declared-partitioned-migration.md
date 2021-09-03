---
description: >-
  This page contains a step by step guide for the migration of Profile Store
  from non-partitioned to partitioned.
---

# Profile Store migration to Declarative Partitioning

## Steps

Rename the existing profilestore table and create new declared partitioned profilestore table -

```sql
ALTER TABLE profilestore rename to profilestore_old;

CREATE TABLE profilestore (
    correlation_id character varying,
    profile_type character varying DEFAULT '_d'::character varying,
    path character varying,
    pit character varying DEFAULT '_latest'::character varying,
    value jsonb NOT NULL,
    certainty real NOT NULL DEFAULT 1.0,
    grain_type character(1) NOT NULL,
    inserted bigint NOT NULL,
    ttl character varying NOT NULL DEFAULT 'P100Y'::character varying,
    reader character varying NOT NULL DEFAULT '_auth'::character varying,
    origin character varying,
    ttn character varying NOT NULL DEFAULT 'P100Y'::character varying,
    CONSTRAINT profilestore_pkey PRIMARY KEY (correlation_id, profile_type, path, pit)
)PARTITION BY LIST (profile_type);
```

Select all distinct `profile_type` from `profilestore_old` table and then Create partitioned table with the each partitioned value profile\_type from the Distinct value -

```sql
SELECT DISTINCT profile_type from profilestore_old;

CREATE TABLE profilestore_device PARTITION OF profilestore FOR VALUES IN ('_device')
CREATE TABLE profilestore_mathematics PARTITION OF profilestore FOR VALUES IN ('mathematics')
CREATE TABLE profilestore_statistics PARTITION OF profilestore FOR VALUES IN ('statistics')
```

Create indexes on the parent table profilestore -

```sql
CREATE UNIQUE INDEX profilestore_pkey ON profilestore(correlation_id text_ops,profile_type text_ops,path text_ops,pit text_ops);
CREATE INDEX profile_type_idx ON profilestore(profile_type text_ops);
CREATE INDEX profilestore_inserted_idx ON profilestore(inserted int8_ops);
CREATE INDEX profilestore_path_idx ON profilestore(path text_ops);
CREATE INDEX profilestore_time_to_act ON profilestore((profile_time_to_act(inserted, ttl)) int8_ops);
```

Once the tables are created we can try to insert records using -

```sql
INSERT INTO "public"."profilestore"(correlation_id,profile_type,path,pit,value,certainty,grain_type,inserted,ttl,reader,origin,ttn)
SELECT correlation_id,profile_type,path,pit,value,certainty,grain_type,inserted,ttl,reader,origin,ttn FROM profilestore_old;
```

Then we can select the records with -

```sql
SELECT * from profilestore where profile_type = 'mathematics';
SELECT * from profilestore where profile_type = '_device';
SELECT * from profilestore where profile_type = 'statistics';
```

