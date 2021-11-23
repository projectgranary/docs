# \[Projects] Use Case Migration Concept

## Create your project

Each use case needs to create a project with the Projects API.

If a project is created ALL objects beloging to this project (Event Types, Harvesters, and Belts) need to be moved to this project (via database `update` statements) as well. Otherwise Granary APIs cannot deploy a project's data pipelines any longer.

{% hint style="warning" %}
If a project decides to move its objects to its own project scope, it is currently not possible to access event types from other projects anylonger. This means if projects rely on reading data from event types owned by different projects, all of the objects must reside in the `global` project scope for now. This will change with future Granary versions by shipping an event type sharing capability.
{% endhint %}

## Event Types

When starting Granary 1.2's version of Harvester API (which includes the Event Type API endpoints), this SQL script is automatically executed:

```sql
UPDATE public.event_types
    SET project_name = 'global'
WHERE
    project_name is NULL;
ALTER TABLE public.event_types ALTER COLUMN project_name SET NOT NULL;
```

This means that all existing event types (possible types are "data\_in", "ttl", or "ttn") are assigned to the `global` project by default. To enable the full feature set of Granary 1.2, the following migrations need to be carried out:

### Existing type "data\_in"

The Project API generates a database schema per project. This schema is supposed to be the only place where project members have read access to on the database. To enable data acess for them, an admin user needs to carry out two steps:

#### &#x20;1. Move to project scope (if other than `global` desired)

```sql
UPDATE public.event_types
    SET project_name = '<your project name' 
WHERE name = '<your technical event type name>';

UPDATE public.persisters
    SET project_name = '<your project name' 
WHERE event_type_name = '<your technical event type name>' AND
    eventstore_type = 'pg';
```

#### 2. Create view on Event Store in project's database schema

```sql
CREATE VIEW <your project name>.<your technical event type name> AS 
SELECT 
    * 
FROM 
    public.eventstore e 
WHERE 
    e.event_type = '<your technical event type name>';
```

### Existing type "ttl" and "ttn"

The [Profile Store Reaper](../../developer-reference/dataflow/profile-store/reaper.md) creates automatically event types of type "ttl" and "ttn". In Granary 1.2 these will be created in `global` project for now. When a project moves to its own project scope, the "ttl" and "ttn" event types belonging to this project must be moved. Thus, an admin user needs to carry out this `update` statement:

```sql
UPDATE public.event_types 
    SET project_name = '<your project name' 
WHERE name = '<your technical event type name>';
```

{% hint style="warning" %}
If you decide to move your existing "ttl" or "ttn" event types to your project scope (or create new ones in your project scope), please make sure to invite the Reaper's technical user in Keycloak to your project as "viewer" as well.
{% endhint %}

### New type "profile"

Granary 1.2 promotes profile types in Profile Store to become first class citizens in the metadata layer. To achieve this, all existing profile types need to be registered at the Event Type API.

{% hint style="warning" %}
Event Types have certain naming restrictions that must also be adhered to when registering existing profile types. See [Hints on Naming Event Types](../../learning-grnry-1/data-in/best-practices-1/hints-on-naming-of-harvesters-and-event-types.md#event-types).
{% endhint %}

This can either be achieved by calling the Event Type API:

```bash
curl -X POST --location "http://{{host}}/projects/<your project name>/event-types" \ 
-H "Content-Type: application/json" \ 
-H "Authorization: {{token}}" \ 
-d "{ \"displayName\": \"<your profile type name>\", \"type\": \"profile\" }"
```

or by executing these two SQLs with an admin user:

#### 1. Create profile type entry

```sql
INSERT
	INTO
	event_types(name,
	version,
	correlation_id_expression,
	event_id_expression,
	timestamp_expression,
	display_name,
	created_at,
	project_name)
VALUES('<your profile type name>',
1,
'#randomUUID()',
'#randomUUID()',
'#nowMillis()',
'profile',
EXTRACT(epoch
FROM
LOCALTIMESTAMP),
'<your project name>');
```

#### 2. Create view on Profile Store in project's database&#x20;

```sql
CREATE VIEW <your project name>.<your profile type name> AS 
SELECT 
    * 
FROM 
    public.profilestore p
WHERE 
    p.profile_type = '<your profile type name>';
```

## Harvesters

All existing Harvesters will be moved to the `global` project automatically. When a project moves to its own project scope, an admin user needs to carry out this `update` statement:

```sql
UPDATE public.harvesters
    SET project_name = '<your project name' 
WHERE name = '<your harvester name>';
```

## Belts

All existing Belts will be moved to the `global` project automatically. When a project moves to its own project scope, an admin user needs to carry out this `update` statement:

```sql
UPDATE public.belts
    SET project_name = '<your project name' 
WHERE id = '<your belt id>';
```

## Segments

See dedicated page for DBT Segments.
