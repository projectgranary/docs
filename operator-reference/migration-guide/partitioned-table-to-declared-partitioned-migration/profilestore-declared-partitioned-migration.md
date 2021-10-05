---
description: >-
  This page contains a step by step guide for the migration of Profile Store
  from non-partitioned to partitioned.
---

# Profile Store migration to Declarative Partitioning

## Steps

1\) Create new ProfileStore table `profilesore_new`with declared partitioned by List`profile_type`:

```sql
CREATE TABLE profilestore_new (
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

2\) Select all distinct `profile_type` from `profilestore` table and then create partitioned tables with each value of `profile_type` from the distinct values:

```sql
SELECT DISTINCT profile_type from profilestore;

CREATE TABLE profilestore_device PARTITION OF profilestore_new FOR VALUES IN ('_device')
CREATE TABLE profilestore_mathematics PARTITION OF profilestore_new FOR VALUES IN ('mathematics')
CREATE TABLE profilestore_statistics PARTITION OF profilestore_new FOR VALUES IN ('statistics')
...
```

3\) Create indexes on the parent table `profilestore_new` :

```sql
CREATE UNIQUE INDEX profilestore_pkey ON profilestore_new(correlation_id text_ops,profile_type text_ops,path text_ops,pit text_ops);
CREATE INDEX profile_type_idx ON profilestore_new(profile_type text_ops);
CREATE INDEX profilestore_inserted_idx ON profilestore_new(inserted int8_ops);
CREATE INDEX profilestore_path_idx ON profilestore_new(path text_ops);
CREATE INDEX profilestore_time_to_act ON profilestore_new((profile_time_to_act(inserted, ttl)) int8_ops);
```

4\) To avoid data courruption, Profile Updater needs to be stopped now.

5\) Once the tables are created, we can try to insert records for one by one `profile_type` using following command:

```sql
INSERT INTO "public"."profilestore_new"(correlation_id,profile_type,path,pit,value,certainty,grain_type,inserted,ttl,reader,origin,ttn)
SELECT correlation_id,profile_type,path,pit,value,certainty,grain_type,inserted,ttl,reader,origin,ttn FROM profilestore where profile_type = 'mathematics';

INSERT INTO "public"."profilestore_new"(correlation_id,profile_type,path,pit,value,certainty,grain_type,inserted,ttl,reader,origin,ttn)
SELECT correlation_id,profile_type,path,pit,value,certainty,grain_type,inserted,ttl,reader,origin,ttn FROM profilestore where profile_type = '_device';

...
```

6\) Finally, we can select the records from the partitioned table with:

```sql
SELECT * from profilestore_new where profile_type = 'mathematics';
SELECT * from profilestore_new where profile_type = '_device';
SELECT * from profilestore_new where profile_type = 'statistics';
```

7\) Rename the table `profilestore` to `profilestore_original` and then rename the `profilestore_new` to `profilestore` table. Before doing so, ensure that there are not open connections to the original table.

```sql
ALTER TABLE profilestore rename to profilestore_original;
ALTER TABLE profilestore_new rename to profilestore;
```

