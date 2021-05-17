# Tutorials & Solutions

## Tutorials

To ease the entry to Granary application \(GrApp\) development, we prepared two tutorials. The first one is directed to Analysts using Granary UI where as the second one targets developers who are familiar using JSON and API endpoints with Postman.

### Tutorial 1: COVID-19 Quote Calculation

In this tutorial, we will create a GrApp to create Life Insurance Quotes for Leads. With Granary we can correlate incoming Quote requests in real time with current COVID-19 data, such delivering improved personalisation.

### Tutorial 2: Onboard Tracking Data

In this tutorial, we will set up a Snowplow tracking GrApp which counts customer sessions.

## Solution Templates

Apart from the tutorials, you can also look into our [GrApp Solution Templates](https://github.com/syncier/grnry-samples/tree/master/grapp-solution-templates):

### Learn how to whitelist your incoming Data

To meet certain constraints \(e.g., GDPR compliance\), Data cannot be persisted just as it comes in. Fields must be chosen explicitly via whitelist before writing them to your database.

{% embed url="https://github.com/syncier/grnry-samples/tree/master/grapp-solution-templates/json-node-whitelisting-grapp" caption="" %}

### Learn how to create personalised views on your Data

Often one representation of your sales landing pages is not enough. Personalize it based on your customers behavior!

{% embed url="https://github.com/syncier/grnry-samples/tree/master/grapp-solution-templates/personalization-grapp" caption="" %}

### Learn how to integrate external Systems

To integrate with a CRM like Salesforce, data must be transformed and imported. Fields must be mapped to the corresponding fields defined in the CRM.

{% embed url="https://github.com/syncier/grnry-samples/tree/master/grapp-solution-templates/salesforce-integration-grapp" caption="" %}

### Import Solutions into Granary

Instead of deploying each Solution Template yourself, you can quickly onboard the finished GrApp with the [GrApp Importer](../staging-of-data-pipelines.md#3-running-the-import-job).

