---
description: >-
  This is to explain how to create use profiles with the personal information
  like first name, last name, age, country code etc.
---

# Pipeline 1: Create Profiles for new Leads

## 1. Creating Lead Event Type

{% hint style="info" %}
In Granary, [Event Types](../../data-in/how-to-run-a-harvester/event-types.md) are the central entity to type incoming raw data.
{% endhint %}

Select the project from left side pane which will open following page:

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 10.24.29 (1).png>)

Click on 'Create' button under Event Types from the option and it will land to the following page :

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 10.28.24.png>)

After adding all the fields we can click on 'Create Event Type' button.

Here the configurations, you need to copy:

> Select Type: `Data in`
>
> Name: `RealTimeET-<yourName>`
>
> Event ID Expression: `#randomUUID()`
>
> Correlation ID Expression: `#safeJsonPath(payload,'cid')?:'NO_CORRELATION_ID'`

{% hint style="info" %}
Here, `‘cid’` is a part of the Snowplow event, we will be passing to Granary to test our pipeline. See a sample Snowplow event at the end of this page.
{% endhint %}

And click on button ‘Create Event Type’. Start the persister `pg` once the event type gets created select `start` from the drop down.

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 10.35.35.png>)

The same way, we need to create an Event Type that represents your transformed data as a [profile](../../../developer-reference/dataflow/event-type.md#profile):&#x20;

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 13.58.22.png>)

Select type `Profile` and after adding all details, we can click on 'Create Event Type' button.

Here the configurations, you need to copy:

> Select Type: `Profile`
>
> Name: `profileET-<yourName>`

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 14.01.29.png>)

## 2. Configure Harvester

{% hint style="info" %}
In Granary, [Harvesters](../../data-in/how-to-run-a-harvester/harvesters.md) are the abstraction for data ingestion pipelines.
{% endhint %}

Again click on the `Project name` from the left side pane for the Project Overview window referred from Step 1.

Click on 'Create' button under Harvesters which will open the following page:

Provide the name of Harvester including a suffix with your name. Choose the Source Type '[Grnry Topic](../../../developer-reference/dataflow/data-in/source-types.md#topic-source)' with the latest version `1.2.0`.

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 17.27.30.png>)

Configure the Source Type – select the default configuration as it is. I.e. we are reading data from the Snowplow API endpoint.

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 10.44.13.png>)

On the next page, select the Event Type `RealTimeET-<yourName>` which we have created above with the `latest` version from the drop down.

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 10.47.39.png>)

Also, we need to add a transform script like:

![](<../../../.gitbook/assets/Screenshot 2022-01-28 at 10.46.21.png>)

```groovy
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import java.util.logging.Logger

// deserialize thrift format
snowplowEvent = SnowplowSerDe.deserialize(payload)

String appId = 'Covid19'
String useCase = 'CostByCountry'
String personProfile = '1'
String body = snowplowEvent.getBody()
println(body)

filter(appId, useCase, personProfile, body)

def filter(appId, useCase, personProfile, body){
    if (hasJsonStructure(body)){
        //parse full snowplowEventBody
        JsonSlurper jsonSlurper = new JsonSlurper()
        def snowplowBody = jsonSlurper.parseText(body)

        if (snowplowBody?.appid?.equals(appId) &&
            snowplowBody?.usecase?.equals(useCase) &&
            snowplowBody?.personprofile?.equals(personProfile)) {
            Logger logger = Logger.getLogger('Logger')
            logger.info('Save event for ' + appId + ': ' + body)
            return body
        }

        // event did not have the right values -> skip
        return null
    }

    // event did not have the right format -> skip
    return null
}

def hasJsonStructure(String body){
    if (body == null){
        return false
    }
    cleaned_body = body.trim()
    return cleaned_body.startsWith('{') && cleaned_body.endsWith('}')
}
```

In the transform script, we are parsing the data which we are getting from the Snowplow event. Primary goal of the script is to transform the Snowplow events from Thrift to JSON. The filter function at line 16 getting called to compares the values of `appId`, `useCase` and `personProfile = 1`, if the event contains the `personProfile` flag as `1`, then it will processed within our pipeline. Otherwise, it will be dropped.

{% hint style="info" %}
Check the Harvester's [best practice section](../../data-in/best-practices-1/) for more scriptable transform examples.
{% endhint %}

Once the Harvester is created, It is stopped in state initially, from the right side of the screen we can see the drop down using that we need to start the Harvester.

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 17.25.31.png>)

After harvester started we can see the state is running and can search the harvester by its name:

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 17.23.42.png>)

## 3. Transform events with the Belt

{% hint style="info" %}
In Granary, a Belt is a function runtime that transforms raw events to valuable information according to business requirements. Check the [Belt runtime reference](../../../developer-reference/dataflow/belt-extractor.md) for a documentation of all its features.
{% endhint %}

Again click on the `Project name` from the left side pane for the Project Overview window referred from Step 1.

Click on 'Create' button under Belts which will open the following page:

Specify the name of Belt `RealTime-Belt-<yourName>` and select the Event Type `RealTimeET-<yourName>` created above as input. On the output side, select the created Profile Event Type named `profileET-<yourName>` in the latest version.

![](<../../../.gitbook/assets/Screenshot 2022-02-01 at 11.30.57.png>)

Once the Belt is created, it looks like this:

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 15.26.20.png>)

We can use Belt configuration parameters:

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 15.27.17.png>)

Also, we need to add a transformation function in Belt:

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 15.33.34.png>)

```python
def execute(event_headers, event,profile=None):   
    profile_corr_id=event_headers['grnry-correlation-id']
    person_name=event['FirstName']+' '+event['LastName']
    person_age=event['Age']
    country=event['Country']
    update_value=event['value']
    updates=[]
    name_dictionary={'Age [Y]':person_age,'Name':person_name,'Country':country}
    updates.append(getUpdatedPersonalDetailsObj(profile_corr_id,'Name',name_dictionary))
    updates.append(getUpdatedPersonalDetailsObj(profile_corr_id,'Age [Y]',name_dictionary))
    updates.append(getUpdatedPersonalDetailsObj(profile_corr_id,'Country',name_dictionary))
    return updates

def getUpdatedPersonalDetailsObj(correlationId,nameString,name_dictionary):
    reader='_all'
    nameStringArray=[nameString]
    operation='_set'
    profile_type='profileET-<yourName>'
    updatedObject=Update(correlationId,nameStringArray,operation=operation)
    updatedObject.set_value(name_dictionary[nameString],reader=reader)
    updatedObject.set_type(profile_type)
    return updatedObject
```

The primary goal of the Belt's transformation function is to model a Profile based on incoming raw events by creating Profile Update objects. In line 10, we create a dictionary which contains `Age`, `Person Name`, and `Country`. These are the keys of the attributes of our Profile. In Granary we call them Grain paths. These details get appended to the Profile Update object using `getUpdatedPersonalDetailsObj()` in line 11ff. At line no 18, **profile\_type** is defined with the name of your output type, i.e. `profileET-<yourName>`. In line 21, the profile gets created using the values getting passed.

{% hint style="info" %}
Check the reference of available [Profile Update operations](../../../developer-reference/dataflow/profile-store/#component-profile-updater) as well as the Belt's [best practice section](../../get-data-out-of-granary/best-practices/) for more transformation function examples.
{% endhint %}

Start the Belt and check if it is in running state:

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 15.35.34.png>)

## 4. Validate with Profile Explorer

To test the pipeline, send a HTTP `POST` request with the following JSON body to the Snowplow API endpoint of demo environment (`<host>/api/com.snowplowanalytics.snowplow/tp2`):

```javascript
{
 "personprofile":"1",
 "appid":"Covid19",
 "usecase":"CostByCountry",
 "cid":"200909095",
 "FirstName" : "Steve",
 "LastName" : "Smith",
 "Age" : "38",
 "Country" :"SA",
 "operation" : "_set",
 "value" : "12"
}
```

{% hint style="info" %}
You can send this event using Granary's Postman collection. See Prerequisits.
{% endhint %}

Once the Snowplow event is triggered, our Harvester will ingest the events into Granary. Then the events will be transformed via our Belt in Granary. When the Belt transforms the events, the grains (using Profile Updates) will get inserted into the Profile Store. Finally, we can retrieve the grains using Profile Explorer.

In Granary UI's Event Browser, we can interactively follow the journey of events through Granary. After the events received at Harvester, we can see them in the 'Data Out' Event Browser:

![](<../../../.gitbook/assets/Screenshot 2022-01-31 at 17.21.19.png>)

After the events are received at Belt, the transform function is executed and Profile Updates are written to the destination topic 'profile-update' which we can see in the 'Data Output' Event Browser:

![](<../../../.gitbook/assets/Screenshot 2022-02-01 at 11.43.57.png>)

The paths "Age \[Y]", "Country", "Name" (first name + last name) with the corresponding values passed get inserted into Profile Store. Select 'Explore Profile' section on the left hand side. There we can see that the profile will look like this:

While searching the profile in Profile Explorer we need to specify :

> Profile Type: profileET-\<yourName>
>
> Correlation id: cid value from the postman request

![](<../../../.gitbook/assets/Screenshot 2022-02-01 at 11.36.28.png>)

{% hint style="info" %}
Naming of all components can be any valid string.
{% endhint %}
