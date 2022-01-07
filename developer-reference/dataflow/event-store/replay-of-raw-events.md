# Replay of Raw Events

## Event Replay

Event Replay enables [belts](../belt-extractor.md) to start at a certain point in time in the past and to resume consuming the live event feed after reprocessing the past events.

As depicted in the raw data zone chart  above, the replay data flow uses the component **Event Feeder** which reads raw events from the Event Store. The feeder emits those typed events to a temporary Kafka topic which can be consumed by the Belt extractor runtime.

### Event Feeder

Event feeder will create a batch job to read events based on configurable event type and send them to a user-defined Kafka topic. How many events to be ingested at every DB batch read is configurable. Spring batch creates metadata tables to control the job instances created. At that moment, the metadata are stored in the (main) database in tables prefixed with "BATCH\_".

| Parameter (env variable name)                | Description                                                                  | Example Value       |
| -------------------------------------------- | ---------------------------------------------------------------------------- | ------------------- |
| EVENTSTORE\_HOSTNAME                         | hostname of the postgres instance                                            | `grnry-pg`          |
| EVENTSTORE\_PORT                             | port of the postgres instance                                                | `5432`              |
| EVENTSTORE\_DB\_NAME                         | database name on postgres instance                                           | `postgres`          |
| IO.GRNRY.EVENTFEEDER.DATASOURCE-TABLE-NAME   | table name on postgres instance                                              | `public.eventstore` |
| IO.GRNRY.EVENTFEEDER.DATASOURCE-USER         | username on postgres instance                                                | `postgres`          |
| IO.GRNRY.EVENTFEEDER.DATASOURCE-PASSWORD     | password on postgres instance                                                | `xxx`               |
| IO.GRNRY.EVENTFEEDER.DATASOURCE-SSL          | Whether database connection should use SSL                                   | `false`             |
| IO.GRNRY.EVENTFEEDER.DATASOURCE-SSL-VALIDATE | Whether server certificate should be validated at secure database connection | `false`             |
| SPRING.KAFKA.BOOTSTRAP-SERVERS               | Name of kafka service to connect to                                          | `grnry-kafka`       |
| IO.GRNRY.EVENTFEEDER.OUTPUT-TOPIC            | Name of temporary output topic                                               | `snowplow-temp`     |
| IO.GRNRY.EVENTFEEDER.EVENT-TYPE              | Batch size for DB read                                                       | `100`               |
| ENCRYPTION\_MODE                             | Whether raw events should be encrypted in temporary output  topic            | `false`             |



### Replay Scenario 1 - Deploy new Belt, start few days back, no event feeding

*   _1st Deployment:_ Belts consume their Kafka source topic not from offset `latest` but from a given offset, see [Belt API ](../../api-reference/belt-api.md)parameter `offset`.

    **Caution:** This only works for the first deployment of a belt. If data should be replayed from an existing belt, a duplicated belt with a new name needs to be created to manipulate the `offset`.
* No previous grains must be invalidated.

### Replay Scenario 2 - Deploy new belts, start on past event data

{% hint style="warning" %}
Replay Scenario 2 builds on the assumption that these two constraints matter to you:

1. The raw events should be replayed in the exact same order as they occurred originally.
2. The replayed raw events should be processed exactly once.

If these two constraints do **not** matter to you, simply replay the raw events to the `grnry_data_in_*` topic of your original data-in event type and let your running belt consume the replayed raw events. Hand-over offsets and deployments detailed below can be ignored in this case.
{% endhint %}

* Event Feeder reads raw events from Event Store and emits them to a temporary topic, settings see above.
* Event Feeder also determines a **Hand-over offset** to switch from temporary to live topic.
* _1st Deployment:_ Belts consume temporary topic from `earlist`. To achieve this, use [Belt API ](../../api-reference/belt-api.md)parameter `offset` with zeros for all partitions.

{% hint style="info" %}
Wait until processing of past events has been finished before starting with _2nd Deployment_.
{% endhint %}

* _2nd Deployment:_ Belts consume their live source topic not from offset `latest` but from the **Hand-over offset**, see [Belt API ](../../api-reference/belt-api.md)parameter `offset`.

{% hint style="info" %}
_2nd Deployment_ needs to be a new Belt definition in Belt API
{% endhint %}

* No previous grains must be invalidated.
