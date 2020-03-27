---
description: >-
  On these pages, we describe how you can use data within Granary. We explain
  how to correctly model the data and how to transform the data from one store
  to another.
---

# Transform Data In Granary

## Profile Modeling

The [Profile Store](../../developer-reference/dataflow/profile-store/#table-profilestore) is at the core of Granary. Here, you model and design how your data is structured. You define which data belongs together and how you want to evaluate it later on. A _profile_ is the core of one real world entity, such as a customer. Within this profile, everything directly related to this entity is stored. For a customer this could be information such as the date of birth, the name, or the current address. As you can see, from this number of items, it is not necessary to normalize the data within your profiles. Instead, we encourage you to write the important data directly to the nodes.

## Belts

The transform step from the Event Store to the Profile Store within Granary is being implemented by so called Belts. Each belt receives data from multiple Kafka Topics, processes it and writes them into an output topic. In most cases, this output topic is then used by a profile updater to write data into the ProfileStore. Documentation about the Belt Extractor, how to design a belt can be found [here](../../developer-reference/dataflow/belt-extractor/) or if you like you may as well use the Belt API. You can find it's documentation [here](../../developer-reference/api-reference/belt-api.md).

### 

