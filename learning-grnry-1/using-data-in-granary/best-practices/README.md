---
description: 'On this page, we are going to discuss best practices for the usage of BELTS.'
---

# Data Modeling Best Practices

### Guiding Principles

To model data within GRNRY, there are some guiding principles you should keep in mind:

* If you have one physical entity \(e.g. a customer\) and different IDs which are used to identify this customer, decide which ID is the leading one and resolve dependencies between IDs to put everything into the leading ID. This leading ID is called `correlation_id`.
* Use the `profile_type` to model different real world entities, such as customer, contract, campaign. They might share the same leading ID, though.
* Consider the combination of `correlation_id` and `profile_type` to be an _entity_ in the sense of [Domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design#Building_blocks).
* Try to avoid having multiple belts writing the same grains.
* Distinguish different semantic areas such as mobile web data from master data by using different fragments, e.g. _/masterdata/name_, _/masterdata/dateOfBirth_, _/mobile\_settings/no\_ads_

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

The Corrleation ID is the unique ID which is used to identify a profile type. The profile type can be e.g. a customer or contract. The important thing about the Correlation ID is that, up to now, this is the only way you can get information about a certain profile from the database via, e.g. the [Profile API](../../../developer-reference/api-reference/profile-store-api.md). So in order to retrieve information from the [Profile Store](../../../developer-reference/dataflow/profile-store/) you must know the corresponding Correlation ID. Hence, the choice of Correlation ID is very important.

In addition, the correlation id is used, e.g. in the [Event Store](../../../developer-reference/api-reference/event-store-api.md), where you can check which events have contributed to the creation of a certain correlation id.

#### A sample model

After answering these questions from above, you should have a good overview on what your data is like. Now, you can start modeling the data. We recommend a structure similar to the ER diagram. However, we put _grains_ into the center of it. A _grain_ represents a certain piece of information about a profile, i.e. a real world entity. A sample diagram looks like this:

![Sample how you could model the profile store](../../../.gitbook/assets/grafik%20%286%29.png)

As you can see the large boxes are profiles. These profiles have _fragments_ which store specific information related to a certain use case or target entity. We also store the Correlation ID here to easily see whether we have the required information to store information in this profile as well. The arrows between different profiles show that there are relationships between the different entities.

For simplicity, at the top of the image, there is also the table structure of the Profile Store. This should help you keeping in mind which kind of data is stored within the database. The `correlation_id` is the unique identifier of a profile. The `profile_type` is what distinguishes several ids of different real world entities from each other. The profile types in this sample are _customer_ and _contract_. The `path` is where the data is stored. For example the name of a customer is stored in the path _/customer/name_. And `pit` just gives the information whether this piece of information is still valid.

Knowing all this, you should be able to start creating your Profile Store.

### Tips & Tricks

Finally, here are some tips and tricks on how to model the profile store. We provide certain additional options to model profiles. These can be really helpful to model the profile store.

* Link different entities together using the semantic  _\_id@profiletype@profilestore_
* Make use of arrays and counters
  * [Arrays ](../../../developer-reference/dataflow/profile-store/#arrays)help you in storing multiple information within one grain, such as a number of favourite colours or certain preferences.
  * [Counters](../../../developer-reference/dataflow/profile-store/#counter) help you in keeping track of countable events, such as how many times a certain website has been visited. You should be aware that counters do not necessarily have to be integers.

