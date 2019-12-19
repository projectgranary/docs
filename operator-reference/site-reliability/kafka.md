# Kafka

## Tuning Kafka Retention Periods

In case you have to comply to any e.g. data retention policies you might have to adjust the time after which Kafka is deleting log segments from its persistence volume.

#### Default Topic Configuration

**retention.ms:** Kafka deletes all closed log segments after 7 days.  
**segment.bytes:** A log segment is closed after reaching 1 GB in size.

Log segments that are not closed before the retention period are kept for another 7 days.

#### Adjust **Cluster** Configuration

Granary is using confluent-kafka helm charts.   
To override any default configuration for the cluster you can add them here [https://github.com/confluentinc/cp-helm-charts/blob/master/charts/cp-kafka/values.yaml\#L37](https://github.com/confluentinc/cp-helm-charts/blob/master/charts/cp-kafka/values.yaml#L37).

**offsets.retention.minutes:** Apply according to your data retention policies.  
**log.roll.ms:** Closes a log segment after the provided time.

#### Adjust Topic Configuration

If you want to overwrite the cluster configuration for a specific topic you can use e.g. Kafka Manager.

**retention.ms:** Overrides offsets.retention.minutes.  
**segment.ms:** Overrides log.roll.ms.

For more information feel free to check the Kafka [documentation](https://kafka.apache.org/documentation/).



## Disaster Recovery Plan

t.b.d. see \#118

### Enable Elasticity

* Choose storage
* Test storage extension

### Monitoring to avoid disconnected Brokers

Todo: Discuss reasons for disconnection. Add description what/why to monitor.

### Recover from HDD full

Todo: Add text.



