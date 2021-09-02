---
description: >-
  This page explained the step by step guild of migration of ProfileStore
  Non-partitioned to Declared-partitioned.
---

# ProfileStore Declared-partitioned migration

## Steps

Rename the existing profilestore table and create new declared partitioned profilestore table -

```text
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

Create partitioned table with the each partitioned value profile\_type -

```text
CREATE TABLE profilestore_device PARTITION OF profilestore_dec_partition FOR VALUES IN ('_device')
CREATE TABLE profilestore_mathematics PARTITION OF profilestore_dec_partition FOR VALUES IN ('mathematics')
CREATE TABLE profilestore_statistics PARTITION OF profilestore_dec_partition FOR VALUES IN ('statistics')
```

Create indexes on the parent table profilestore -

```text
CREATE UNIQUE INDEX profilestore_pkey ON profilestore_dec_partition(correlation_id text_ops,profile_type text_ops,path text_ops,pit text_ops);
CREATE INDEX profile_type_idx ON profilestore_dec_partition(profile_type text_ops);
CREATE INDEX profilestore_inserted_idx ON profilestore_dec_partition(inserted int8_ops);
CREATE INDEX profilestore_path_idx ON profilestore_dec_partition(path text_ops);
CREATE INDEX profilestore_time_to_act ON profilestore_dec_partition((profile_time_to_act(inserted, ttl)) int8_ops);
```

Once the tables are created we can try to insert records using -

```text
INSERT INTO "public"."profilestore_dec_partition"("correlation_id","profile_type","path","pit","value","certainty","grain_type","inserted","ttl","reader","origin","ttn")
VALUES
('0000119e-7f9b-4796-b3b6-8fdd1be79176','mathematics','/test','_latest','"test14409"',1,'t',1627380326019,'P100Y','_all','/belts/9','P100Y');

INSERT INTO "public"."profilestore_dec_partition"("correlation_id","profile_type","path","pit","value","certainty","grain_type","inserted","ttl","reader","origin","ttn")
VALUES
('00002x4c-e504-4b34-8aa1-be209e636948','_device','/test','_latest','"test9300"',1,'t',1627325540060,'P100Y','_all','/belts/9','P100Y');

INSERT INTO "public"."profilestore_dec_partition"("correlation_id","profile_type","path","pit","value","certainty","grain_type","inserted","ttl","reader","origin","ttn")
VALUES
('00004s32d-82c2-4772-8b74-316caad85b50','statistics','/test10','_latest','"test19508"',1,'t',1627317871756,'P100Y','_all','/belts/9','P100Y');
```

Then we can select the records with -

```text
SELECT * from profilestore where profile_type = 'mathematics';
SELECT * from profilestore where profile_type = '_device';
SELECT * from profilestore where profile_type = 'statistics';
```

