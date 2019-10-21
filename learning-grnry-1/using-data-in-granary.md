---
description: >-
  On these pages, it should be described, how you can use data within Granary.
  We describe how to correctly model the data and how to transform the data from
  one store to another.
---

# Model Data In Granary

## Belts

The transform step from the Event Store to the Profile Store within Granary is being implemented by so called Belts. Each belt receives data from multiple Kafka Topics, processes it and writes them into an output topic. In most cases, this output topic is then used by a profile updater to write data into the ProfileStore. Documentation about the Belt Extractor, how to design a belt can be found [here](../developer-reference/dataflow/belt-extractor.md) or if you like you may as well use the Belt API, where you can find documentation [here](../developer-reference/api-reference/belt-api.md).

## Best Practices for Profile Modeling

The Profile Store is at the core of Granary. Here, you model and design how your data is structured. You define which data belongs together and how you want to evaluate it later on. A _profile_ is the core of one real world entity, such as a customer. Within this profile, everything directly related to this entity is stored. For a customer this could be information such as the date of birth, the name, or the current address. As you can see, from this number of items, it is not necessary to normalize the data within your profiles. Instead, we encourage you to write the important data directly to the nodes.

### How to get started

Like with traditional database modeling, it is important that you actually know, what you want to achieve. You should be able to answer the following questions:

* What do I want to do with the data?
* How do I need to structure the data to support this use case?
* Which other use cases could there be?
* Which IDs are leading in the analysis of my data, meaning by which attributes do I want to structure the data? These IDs are called _Correlation ID_ and uniquely identify the profile.
* Do I have the attributes and if not, how do I get them?
* Are there already similar entities with the same/different IDs? If yes, can they be merged together?

#### Correlation ID

{% hint style="info" %}
The correlation ID is the most important ID in the system as it is used to uniquely identify profiles and related events.
{% endhint %}

The Corrleation ID is the unique ID which is used to identify a profile type. The profile type can be e.g. a customer or contract. The important thing about the Correlation ID is that, up to now, this is the only way you can get information about a certain profile from the database via, e.g. the [Profile API](../developer-reference/api-reference/profile-store-api.md). So in order to retrieve information from the [Profile Store](../developer-reference/dataflow/profile-store/) you must know the corresponding Correlation ID. Hence, the choice of Correlation ID is very important.

In addition, the correlation id is used, e.g. in the [Event Store](../developer-reference/api-reference/event-store-api.md), where you can check which events have contributed to the creation of a certain correlation id.

#### A sample model

After answering these questions from above, you should have a good overview on what your data is like. Now, you can start modeling the data. We recommend a structure similar to the ER diagram. However, we put _grains_ into the center of it. A _grain_ represents a certain piece of information about a profile, i.e. a real world entity. A sample diagram looks like this:

![Sample how you could model the profile store](../.gitbook/assets/grafik%20%283%29.png)

As you can see the large boxes are profiles. These profiles have _fragments_ which store specific information related to a certain use case or target entity. We also store the Correlation ID here to easily see whether we have the required information to store information in this profile as well. The arrows between different profiles show that there are relationships between the different entities.

For simplicity, at the top of the image, there is also the table structure of the Profile Store. This should help you keeping in mind which kind of data is stored within the database. The `correlation_id` is the unique identifier of a profile. The `profile_type` is what distinguishes several ids of different real world entities from each other. The profile types in this sample are _customer_ and _contract_. The `path` is where the data is stored. For example the name of a customer is stored in the path _/customer/name_. And `pit` just gives the information whether this piece of information is still valid.

Knowing all this, you should be able to start creating your Profile Store.

### Guiding Principles

To model data within GRNRY, there are some guiding principles you should keep in mind:

* If you have one physical entity \(e.g. a customer\) and different IDs which are used to identify this customer, decide which ID is the leading one and resolve dependencies between IDs to put everything into the leading ID.
* Try to avoid having multiple belts writing the same grains.
* Distinguish different semantic areas such as mobile web data from master data by using different fragments, e.g. _/masterdata/name_, _/masterdata/dateOfBirth_, _/mobile\_settings/no\_ads_
* Use the profile\_type to model different real world entities, such as customer, contract, campaign

### Tips & Tricks

Finally, here are some tips and tricks on how to model the profile store. We provide certain additional options to model profiles. These can be really helpful to model the profile store.

* Link different entities together using the semantic  _\_id@profiletype@profilestore_
* Make use of arrays and counters
  * [Arrays ](../developer-reference/dataflow/profile-store/#arrays)help you in storing multiple information within one grain, such as a number of favourite colours or certain preferences.
  * [Counters](../developer-reference/dataflow/profile-store/#counter) help you in keeping track of countable events, such as how many times a certain website has been visited. You should be aware that counters do not necessarily have to be integers.

