# Firebase Tracker

## Using Cloud Function triggers for Analytics Events

Cloud Functions is a service by Firebase that allows us to trigger nodejs code in response to certain triggers \(i.e. certain analytics events send by our app, crashlytics events, etc.\). For a full description of triggers please refer to [https://cloud.google.com/functions/docs/concepts/events-triggers](https://cloud.google.com/functions/docs/concepts/events-triggers). First you will need to create some events of the type you want to use as a trigger. Once they appear in the Firebase Analytics Events console you need to mark the respective events as "conversions". PLEASE take note that you can only mark a certain number of event types as conversions, please refer to the Firebase documentation for this number. 

![](../../.gitbook/assets/eventconversiondeclaration%20%281%29.png)

First you need to install the firebase-functions and the various dependencies using npm. Then you will set up your firebase project locally to accept your functions written in nodejs. For guidelines for the setup please refer to [Setting up Firebase Cloud functions](https://docs.grnry.io/~/drafts/-LIkJkVZOwUaX-xi7fBZ/primary/user-guide/firebase-integration-via-cloud-functions/setting-up-firebase-cloud-functions).

Once you are set up you can start writing your nodejs cloudfunctions to translate the incomming firebase analytics events into snowplow events. An example of the functions code is provided below with additional comments.  A file with functions for all auomaticly collected Firebase events can be found here &lt;INSERT LINK&gt;

{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const functions = require('firebase-functions');
const snowplow = require('snowplow-tracker');

const emitter = snowplow.emitter;
const tracker = snowplow.tracker;

const nameSpace = 'alvary-demo';
const appId = 'fb-cloudfunctions';
const domainId = 'aws-eu1.grnry.io';
const protocol = 'https';
const requestMethod = 'POST';

const e = emitter(
    domainId,
    protocol,
    null,
    requestMethod,
    1, // number of queued events needed before request is send to endpoint
    function (error, body, response) { // Callback called for each request
        if (error) {
            console.log("Request failed");
        }
    },
    { maxSockets: 6 }
);

// initialising the snowplow tracker
const t = tracker([e], nameSpace, appId, false);

// cloud function to trigger on screen_view events and send an unstructured snowplow event
// to the enpoint with the event JSON attached as data (including user properties) 
exports.logScreenView = functions.analytics.event('screen_view').onLog((event) => {
t.trackUnstructEvent(event);
return null; });

```
{% endcode-tabs-item %}
{% endcode-tabs %}

Once you have written your cloud functions you can deploy them using:

```bash
firebase deploy
```

After the deployment when you create the respective firebase analytics events you can check that the functions have been executed without error by checking in he functions tab of the firebase console. The count of executions has only a small latency so you should see almost imidiate results. If you only have recently declared an event type a conversion it might take a while until you see the respective functions triggered.



![](../../.gitbook/assets/coudfunctionsdashboard.png)

#### GRNRY Firebase Event tracking

As the amount of Events to be labeled as conversions is limited \(and therefore the amount of event types to be forewareded to snowplow\) we would advise that if you plan on tracking many different custom events to consolidate them into one GRNRY\_CUSTOM1 event with up to 25 custom keys \(consistent over all events of this type !\). This might however result in less optimal performance of the forewareding cloud function !. See below for an example:

```java
Bundle grnryBundle = new Bundle();
grnryBundle = initBundle(grnryBundle);
grnryBundle.putString("GRNRY_CUSTOM_PARAMETER_1", "Value 1");
grnryBundle.putString("GRNRY_CUSTOM_PARAMETER_2", "Value 2");
grnryBundle.putString("GRNRY_CUSTOM_PARAMETER_3", "Value 3");
FirebaseAnalytics.getInstance(getApplicationContext()).logEvent("GRNRY_CUSTOM1", grnryBundle);
```

#### Firebase User Properties

To hand over informations specific to the app installation with every Firebase Analytics Event it is possible to define Firebase user properties.

```java
FirebaseAnalytics.getInstance(getApplicationContext()).setUserProperty("KEY","VALUE");
```

We would advise you to use two properties for GRNRY specific, first a GRNRUID which should be random UUID and second a GRNRYSESSIONID \(also a random UUID\) that is reset whenever a new session \(specifics to be defined\) is started.

