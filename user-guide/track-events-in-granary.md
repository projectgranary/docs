# Track Online Events

### Overview

Granary is built so that you can send in event level data from _any_ type of digital application, service or device. There is a range of trackers available to enable you to easily collect event-level data from lots of different places.

 When you instrument a tracker you need to set it up in such a way that it responds to the events you need to track by:

1. Seeing those events
2. Assembling a packet of data points that fully describe those events
3. Sending the packet of data representing that event to the collector for processing

This process looks a bit different depending on the tracker you’re implementing. However, the underlying process is the same in all cases - the main thing to consider is - is the event you’re tracking one that you have defined, or that has been defined already in Granary?

### Tracking events that have already been defined

Granary supports a large and growing number of events ‘out of the box’, most of which are fairly standard in an analytics context. Examples of supported events include:

* Page views
* Page pings
* Link clicks
* Form fill-ins \(for the web\)
* Form submissions
* Transactions

For events that Granary natively supports, there is generally a specific API for tracking that event type. For example, if you want to track a page view using the Javascript tracker, you do so with the following Javascript:

```javascript
window.snowplow('trackPageView');
```

 Whereas if you were tracking a pageview in an iOS app using the objective-c tracker, you’d do so like this:

```objectivec
[t1 trackPageView:@"www.example.com" title:@"example" referrer:@"www.referrer.com"];
```

In general, each tracker will have a specific API call for tracking any events that have been defined already, and you should refer to the tracker-specific documentation to make sure that this is set up correctly.

#### **Contexts that are supported out-of-the-box**

Wherever possible, the trackers tries to automatically capture as much contextual data for each event as possible. For example, with the Javascript tracker, the following data fields are automatically captured with every request unless they are disabled:

| dvce\_tstamp | The timestamp on th device that the event was recorded on |
| :--- | :--- |
| os\_timezone | The timezone the client operating system is set to |
| event\_id | A unique identifier for the event |
| domain\_userid | First party cookie ID |
| domain\_sessionidx | Session index based on first party cookie ID |
| dvce\_screenheight | Screen width in pixels |
| br\_viewwidth | Browser view width in pixels |
| br\_viewheight | Browser view height in pixels |
| page\_url | URL of the page on wich the event occurred |
| page\_referrer | URL of the referrer |
| user\_fingerprint | Browser fingerprint |
| br\_lang | Language the browser is set to |
| br\_features\_... | A list of boolean flags to indicate if common plugins are installed e.g. PDF,Quicktime, RealPlayer, Flash, Java, ... |
| br\_colordepth | Browser color depth |
| doc\_width | Width of webpage in pixels |
| doc\_height | Height of webpage in pixels |
| doc\_charset | Document encoding |
| platform | The platform that the event was recorded on, in this case 'web' |
| name\_tracker | The tracker name |
| v\_tracker | The tracker vers |

In addition to the above fields, there are a number of additional optional contexts that you can capture automatically using the Javascript tracker, including:

* [Performance timing](https://github.com/snowplow/snowplow/wiki/1-General-parameters-for-the-Javascript-tracker#predefined-contexts). This provides data on web page load times.
* [Universal Analytics cookie data 12](https://github.com/snowplow/snowplow/wiki/1-General-parameters-for-the-Javascript-tracker#22132-gacookies-context). This provides data read from the Google Analytics cookie, for users running Snowplow alongside Universal Analytics
* [Geolocation context](https://github.com/snowplow/snowplow/wiki/1-General-parameters-for-the-Javascript-tracker#22133-geolocation-context). This will provide data on where a user is, if that user has consented to give that information.

The mobile \(iOS and Android trackers\) also automatically capture a large number of data points with every event, where available:

| os\_type | Operating system type |
| :--- | :--- |
| os\_version | Operating system version |
| device\_manufacturer | The device manufacturer |
| device\_model | The device model |
| carrier | The mobile carrier |
| apple\_idfa | Apple's IDFA \(ID for advertisers\) |
| open\_idfa | The open [IDFA](https://github.com/ylechelle/OpenIDFA) |
| android\_idfa | The Android ID |
| latitude | Devide location latitude |
| longitude | Device location longitude |
| latitude\_longitude\_location\_accuracy | The accuracy of the lat/long measures |
| altitude | Device location altitude |
| altitude\_accuracy | The accuracy for the altitude measure |
| bearing | Direction of device travel |
| speed | Speed with which the device is travelling |

### Tracking events that you’ve defined yourself

 Tracking events where you have defined the schema yourself is straightforward. Before you instrument your tracker, you need to:

1. Make sure you have your Iglu schema repo setup
2. Create a schema for your event type in the repo
3. Have the associated reference to the schema in Iglu. So for example, if your company website URL is `mycompany.com`, and you’ve defined your own `outbound-call-made` event schema, and it is the first version of that schema, then the reference to the schema is `iglu:com.mycompany/outbound-call-made/jsonschema/1-0-0`

 Once that is done you simply need to configure your tracker to record the event using the `track unstructured event` method. So if we were tracking the event using the Python tracker, our code snippet for doing so might look like this:

```python
tracker.track_unstruct_event({
    "schema": "iglu:com.mycompany/outbound-call-made/jsonschema/1-0-0",
    "data": {
        "connected_tstamp": "2015-03-21 17:23:10",
        "disconnected_tstamp": "2015-03-21 17:48:21",
        "reason_for_call": "Response to interest submitted via webform",
        "success": true,
        "order_id": "ab-1903-23904",
        "order_value": "129.44"
    }
})
```

We call the _track unstructured event_ method and pass in a JSON with two fields, a schema field, which tells Granary where the schema for this event can be located in Iglu, and a data field, that includes that actual data that needs to be captured. We call this a [self-describing JSON](http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/), because assuming we have access to Iglu, the JSON contains all the information we need to process it, in the form of the schema.

Each of the trackers includes a _track unstructured event_ method and it is not uncommon to have implementation where nearly all if not all the events tracked have been defined by the company in question, and so are all tracked using this method.

#### Tracking contexts that you’ve defined yourself

Whenever you track any event, using any tracker, you can pass into Granary as many contexts as you want. This gives you the flexibility to pass potentially enormous amounts of data with each event that you capture.

Across all our trackers, the approach is the same. Each context is a [self-describing JSON](http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/). We create an array of all the different contexts that we wish to pass into Granary, and then we pass those contexts in generally as the final argument on any _track_ method that we call to capture the event. \(E.g. _track pageview_, _track structured event_, _track unstructured event_ etc.\) So for example, we can extend the example above to pass in a user and product context:

```python
tracker.track_unstruct_event({
    "schema": "iglu:com.mycompany/outbound-call-made/jsonschema/1-0-0",
    "data": {
        "connected_tstamp": "2015-03-21 17:23:10",
        "disconnected_tstamp": "2015-03-21 17:48:21",
        "reason_for_call": "Response to interest submitted via webform",
        "success": true,
        "order_id": "ab-1903-23904",
        "order_value": "129.44"
    }
}, context=[{
    "schema": "iglu:com.mycompany/customer/jsonschema/1-0-0",
    "data": {
        "name": "Joe Bloggs",
        "address_street": "123 ABC Road",
        "address_town": "my town",
        "address_state": "my state",
        "address_country": "United States of America"
    }
},{
    "schema": "iglu:com.mycompany/product/jsonschema/1-0-0",
    "data": {
        "sku": "1908asdf",
        "name": "product name",
        "list_price": "149.99",
        "discounted_price": "129.44",
        "promotion": "end of season",
        "color": "red"
    }
}])
```



