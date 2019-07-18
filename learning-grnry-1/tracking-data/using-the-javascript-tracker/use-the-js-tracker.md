# Specific Events

## Page Views

Page views are tracked using the `trackPageView` method. This is generally part of the first Snowplow tag to fire on a particular web page. As a result, the `trackPageView` method is usually deployed straight after the tag that also invokes the Snowplow JavaScript \(sp.js\) e.g.

```javascript
<!-- Snowplow starts plowing -->
<script type="text/javascript">
;(function(p,l,o,w,i,n,g){if(!p[i]){p.GlobalSnowplowNamespace=p.GlobalSnowplowNamespace||[];
p.GlobalSnowplowNamespace.push(i);p[i]=function(){(p[i].q=p[i].q||[]).push(arguments)
};p[i].q=p[i].q||[];n=l.createElement(o);g=l.getElementsByTagName(o)[0];n.async=1;
n.src=w;g.parentNode.insertBefore(n,g)}}(window,document,"script","//d1fc8wv8zag5ca.cloudfront.net/2.9.0/sp.js","snowplow_name_here"));

snowplow_name_here('enableActivityTracking', 30, 10);

snowplow_name_here('trackPageView');

 </script>
<!-- Snowplow stops plowing -->
```

### trackPageView

Track pageview is called using the simple:

```javascript
snowplow_name_here('trackPageView');
```

This method automatically captures the URL, referrer and page title \(inferred from the `<title>`tag. If you wish, you can override the title with a custom value:

```javascript
snowplow_name_here('trackPageView', 'my custom page title');
```

`trackPageView` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

Additionally, you can pass a function which returns an array of zero or more contexts to trackPageView. For the page view and for all subsequent page pings, the function will be called and the contexts it returns will be added to the event.

For example:

```javascript
// Turn on page pings every 10 seconds
window.snowplow('enableActivityTracking', 10, 10);

window.snowplow(
  'trackPageView',

  // no custom title
  null,

  // The usual array of static contexts
  [{
    schema: 'iglu:com.acme/static_context/jsonschema/1-0-0',
    data: {
      staticValue: new Date().toString()
    }
  }],

  // Function which returns an array of custom contexts
  // Gets called once per page view / page ping
  function() {
    return [{
      schema: 'iglu:com.acme/dynamic_context/jsonschema/1-0-0',
      data: {
        dynamicValue: new Date().toString()
      }
    }];
  }
);
```

The page view and every subsequent page ping will have both a static\_context and a dynamic\_context attached. The static\_contexts will all have the same staticValue, but the dynamic\_contexts will have different dynamicValues since a new context is created for every event.

## Page Pings

As well as tracking page views, we can monitor whether a user continues to engage with a page over time, and record how he / she digests content on the page over time.

That is accomplished using 'page ping' events. If activity tracking is enabled, the web page is monitored to see if a user is engaging with it. \(E.g. is the tab in focus, does the mouse move over the page, does the user scroll etc.\) If any of these things occur in a set period of time, a page ping event fires, and records the maximum scroll left / right and up / down in the last ping period. If there is no activity in the page \(e.g. because the user is on a different tab in his / her browser\), no page ping fires.

### **enableActivityTracking**

 Page pings are enabled by:

```text
snowplow_name_here('enableActivityTracking', minimumVisitLength, heartBeat);
```

 where `minimumVisitLength` is the time period from page load before the first page ping occurs, in seconds. Heartbeat is the number of seconds between each page ping, once they have started. So, if you executed:

```javascript
snowplow_name_here('enableActivityTracking', 30, 10);

snowplow_name_here('trackPageView');
```

The first ping would occur after 30 seconds, and subsequent pings every 10 seconds as long as the user continued to browse the page actively.

Notes:

* In general this is executed as part of the main Snowplow tracking tag. As a result, you can elect to enable this on specific pages.
* The `enableActivityTracking` method **must** be called _before_ the `trackPageView` method.
* Activity tracking will be disabled if either `minimumVisitLength` or `heartBeat` is not integer. This is to prevent relentless callbacks.

### **updatePageActivity**

You can also trigger a page ping manually with:

```javascript
snowplow_name_here('updatePageActivity');
```

This is particularly useful when a user is passively engaging with your content, e.g. watching a video.

## Ecommerce tracking

Modelled on Google Analytics ecommerce tracking capability, Snowplow uses three methods that have to be used together to track online transactions:

1. **Create a transaction object**. Use `addTrans()` method to initialize a transaction object. This will be the object that is loaded with all the data relevant to the specific transaction that is being tracked including all the items in the order, the prices of the items, the price of shipping and the `order_id`.
2. **Add items to the transaction.** Use the `addItem()` method to add data about each individual item to the transaction object.
3. **Submit the transaction to Snowplow** using the trackTrans\(\) method, once all the relevant data has been loaded into the object.

### **addTrans**

The `addTrans` method creates a transaction object. It takes nine possible parameters, two of which are required:

| **Parameter** | **Description** | **Required?** | **Example value** |
| :--- | :--- | :--- | :--- |
| `orderId` | Internal unique order id number for this transaction | Yes | '1234' |
| `affiliation` | Partner or store affiliation | No | 'Womens Apparel' |
| `total` | Total amount of the transaction | Yes | '19.99' |
| `tax` | Tax amount of the transaction | No | '1.00' |
| `shipping` | Shipping charge for the transaction | No | '2.99' |
| `city` | City to associate with transaction | No | 'San Jose' |
| `state` | State or province to associate with transaction | No | 'California' |
| `country` | Country to associate with transaction | No | 'USA' |
| `currency` | Currency to associate with this transaction | No | 'USD' |

For example:

```javascript
snowplow_name_here('addTrans',
    '1234',           // order ID - required
    'Acme Clothing',  // affiliation or store name
    '11.99',          // total - required
    '1.29',           // tax
    '5',              // shipping
    'San Jose',       // city
    'California',     // state or province
    'USA',            // country
    'USD'             // currency
  );
```

`addTrans` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

### **addItem**

The `addItem` method is used to capture the details of each product item included in the transaction. It should therefore be called once for each item.

There are six potential parameters that can be passed with each call, four of which are required:

| **Parameter** | **Description** | **Required?** | **Example value** |
| :--- | :--- | :--- | :--- |
| `orderId` | Order ID of the transaction to associate with item | Yes | '1234' |
| `sku` | Item's SKU code | Yes | 'pbz0001234' |
| `name` | Product name | No, but advisable \(to make interpreting SKU easier\) | 'Black Tarot' |
| `category` | Product category | No | 'Large' |
| `price` | Product price | Yes | '9.99' |
| `quantity` | Purchase quantity | Yes | '1' |
| `currency` | Product price currency | No | 'USD' |

For example:

```javascript
snowplow_name_here('addItem',
    '1234',           // order ID - required
    'DD44',           // SKU/code - required
    'T-Shirt',        // product name
    'Green Medium',   // category or variation
    '11.99',          // unit price - required
    '1',              // quantity - required
    'USD'             // currency
  );
```

`addItem` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

### **trackTrans**

Once the transaction object has been created \(using `addTrans`\) and the relevant item data added to it using the `addItem` method, we are ready to send the data to the collector. This is initiated using the `trackTrans` method:

```javascript
snowplow_name_here('trackTrans');
```

### **A complete example**

```markup
<html>
<head>
<title>Receipt for your clothing purchase from Acme Clothing</title>
<script type="text/javascript">

  ;(function(p,l,o,w,i,n,g){if(!p[i]){p.GlobalSnowplowNamespace=p.GlobalSnowplowNamespace||[];
  p.GlobalSnowplowNamespace.push(i);p[i]=function(){(p[i].q=p[i].q||[]).push(arguments)
  };p[i].q=p[i].q||[];n=l.createElement(o);g=l.getElementsByTagName(o)[0];n.async=1;
  n.src=w;g.parentNode.insertBefore(n,g)}}(window,document,"script","//d1fc8wv8zag5ca.cloudfront.net/2.5.1/sp.js","snowplow_name_here"));

  snowplow_name_here('newTracker', 'cf', 'd3rkrsqld9gmqf.cloudfront.net');
  snowplow_name_here('enableActivityTracking', 30, 10)
  snowplow_name_here('trackPageView');
  snowplow_name_here('enableLinkClickTracking');

  snowplow_name_here('addTrans',
    '1234',           // order ID - required
    'Acme Clothing',  // affiliation or store name
    '11.99',          // total - required
    '1.29',           // tax
    '5',              // shipping
    'San Jose',       // city
    'California',     // state or province
    'USA'             // country
  );

   // add item might be called for every item in the shopping cart
   // where your ecommerce engine loops through each item in the cart and
   // prints out _addItem for each
  snowplow_name_here('addItem',
    '1234',           // order ID - required
    'DD44',           // SKU/code - required
    'T-Shirt',        // product name
    'Green Medium',   // category or variation
    '11.99',          // unit price - required
    '1'               // quantity - required
  );

  // trackTrans sends the transaction to Snowplow tracking servers.
  // Must be called last to commit the transaction.
  snowplow_name_here('trackTrans'); //submits transaction to the collector

</script>
</head>
<body>

  Thank you for your order.  You will receive an email containing all your order details.

</body>
</html>
```

## Campaign tracking

Campaign tracking is used to identify the source of traffic coming to a website.

At the highest level, we can distinguish **paid** traffic \(that derives from ad spend\) with **non paid** traffic: visitors who come to the website by entering the URL directly, clicking on a link from a referrer site or clicking on an organic link returned in a search results, for example.

In order to identify **paid** traffic, Snowplow users need to set five query parameters on the links used in ads. Snowplow checks for the presence of these query parameters on the web pages that users load: if it finds them, it knows that that user came from a paid source, and stores the values of those parameters so that it is possible to identify the paid source of traffic exactly.

If the query parameters are not present, Snowplow reasons that the user is from a **non paid** source of traffic. It then checks the page referrer \(the url of the web page the user was on before visiting our website\), and uses that to deduce the source of traffic:

1. If the URL is identified as a search engine, the traffic medium is set to "organic" and Granary tries to derive the search engine name from the referrer URL domain and the keywords from the query string.
2. If the URL is a non-search 3rd party website, the medium is set to "referrer". Granary derives the source from the referrer URL domain.

### **Identifying paid sources**

Your different ad campaigns \(PPC campaigns, display ads, email marketing messages, Facebook campaigns etc.\) will include one or more links to your website e.g.:

```markup
<a href="http://mysite.com/myproduct.html">Visit website</a>
```

We want to be able to identify people who've clicked on ads e.g. in a marketing email as having come to the site having clicked on a link in that particular marketing email. To do that, we modify the link in the marketing email with query parameters, like so:

```markup
<a href="http://mysite.com/myproduct.html?utm_source=newsletter-october&utm_medium=email&utm_campaign=cn0201">Visit website</a>
```

For the prospective customer clicking on the link, adding the query parameters does not change the user experience. \(The user is still directed to the webpage at [http://mysite.com/myproduct.html](http://mysite.com/myproduct.html).\) But Snowplow then has access to the fields given in the query string, and uses them to identify this user as originating from the October Newsletter, an email marketing campaign with campaign id = cn0201.

### **Query Parameters**

Granary uses the same query parameters used by Google Analytics. Because of this, Granary users who are also using GA do not need to do any additional work to make their campaigns trackable in Granary as well as GA. Those parameters are:

| **Parameter** | **Name** | **Description** |
| :--- | :--- | :--- |
| `utm_source` | Campaign source | Identify the advertiser driving traffic to your site e.g. Google, Facebook, autumn-newsletter etc. |
| `utm_medium` | Campaign medium | The advertising / marketing medium e.g. cpc, banner, email newsletter, in-app ad, cpa |
| `utm_campaign` | Campaign id | A unique campaign id. This can be a descriptive name or a number / string that is then looked up against a campaign table as part of the analysis |
| `utm_term` | Campaign term\(s\) | Used for search marketing in particular, this field is used to identify the search terms that triggered the ad being displayed in the search results. |
| `utm_content` | Campaign content | Used either to differentiate similar content or two links in the same ad. \(So that it is possible to identify which is generating more traffic.\) |

The parameters are descibed in the [Google Analytics help page](https://support.google.com/analytics/answer/1033863). Google also provides a [urlbuilder](https://support.google.com/analytics/answer/1033867?hl=en)which can be used to construct the URL incl. query parameters to use in your campaigns.

## Ad tracking methods

Snowplow tracking code can be included in ad tags in order to track impressions and ad clicks. This is used by e.g. ad networks to identify which sites and web pages users visit across a network, so that they can be segmented, for example.

Each ad tracking method has a `costModel` field and a `cost` field. If you provide the `cost` field, you must also provide one of `'cpa'`, `'cpc'`, and `'cpm'` for the `costModel` field.

It may be the case that multiple ads from the same source end up on a single page. If this happens, it is important that the different Snowplow code snippets associated with those ads not interfere with one another. The best way to prevent this is to randomly name each tracker instance you create so that the probability of a name collision is negligible. See [Managing multiple trackers](https://github.com/snowplow/snowplow/wiki/1-General-parameters-for-the-Javascript-tracker#25-managing-multiple-trackers) for more on having more than one tracker instance on a single page.

Below is an example of how to achieve this when using Snowplow ad impression tracking.

```markup
<!-- Snowplow starts plowing -->
<script type="text/javascript">

// Wrap script in a closure.
// This prevents rnd from becoming a global variable.
// So if multiple copies of the script are loaded on the same page,
// each instance of rnd will be inside its own namespace and will
// not overwrite any of the others.
// See http://benalman.com/news/2010/11/immediately-invoked-function-expression/
(function(){
  // Randomly generate tracker namespace to prevent clashes
  var rnd = Math.random().toString(36).substring(2);

  // Load Snowplow
  ;(function(p,l,o,w,i,n,g){if(!p[i]){p.GlobalSnowplowNamespace=p.GlobalSnowplowNamespace||[];
  p.GlobalSnowplowNamespace.push(i);p[i]=function(){(p[i].q=p[i].q||[]).push(arguments)
  };p[i].q=p[i].q||[];n=l.createElement(o);g=l.getElementsByTagName(o)[0];n.async=1;
  n.src=w;g.parentNode.insertBefore(n,g)}}(window,document,"script","//d1fc8wv8zag5ca.cloudfront.net/2.5.1/sp.js","sp_pm"));

  // Create a new tracker namespaced to rnd
  window.sp_pm('newTracker', rnd, 'dgrp31ac2azr9.cloudfront.net', {
    appId: 'myApp',
    platform: 'web'
  });

  // Replace the values below with magic macros from your adserver
  window.sp_pm('trackAdImpression:' + rnd,
    '67965967893',            // impressionId
    'cpm',                    // costModel - 'cpa', 'cpc', or 'cpm'
    5.5,                      // cost
    'http://www.example.com', // targetUrl
    '23',                     // bannerId
    '7',                      // zoneId
    '201',                    // advertiserId
    '12'                      // campaignId
  );
}());
</script>
<!-- Snowplow stops plowing -->
```

Even if several copies of the above script appear on a page, the trackers created will all \(probably\) have different names and so will not interfere with one another. The same technique should be used when tracking ad clicks. The below examples for `trackAdImpression` and `trackAdClick`assume that `rnd` has been defined in this way.

### **trackAdImpression**

Ad impression tracking is accomplished using the `trackAdImpression` method. Here are the arguments it accepts:

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `impressionId` | No | Identifier for the particular impression instance | string |
| `costModel` | No | The cost model for the campaign: 'cpc', 'cpm', or 'cpa' | string |
| `cost` | No | Ad cost | number |
| `targetUrl` | No | The destination URL | string |
| `bannerId` | No | Adserver identifier for the ad banner \(creative\) being displayed | string |
| `zoneId` | No | Adserver identifier for the zone where the ad banner is located | string |
| `advertiserID` | No | Adserver identifier for the advertiser which the campaign belongs to | string |
| `campaignId` | No | Adserver identifier for the ad campaign which the banner belongs to | string |

An example:

```text
snowplow_name_here('trackAdImpression:' + rnd,

    '67965967893',             // impressionId
    'cpm',                     // costModel - 'cpa', 'cpc', or 'cpm'
     5.5,                      // cost
    'http://www.example.com',  // targetUrl
    '23',                      // bannerId
    '7',                       // zoneId
    '201',                     // advertiserId
    '12'                       // campaignId
);
```

Ad impression events are implemented as Snowplow unstructured events. [Here](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/ad_impression/jsonschema/1-0-0) is the JSON schema for an ad impression event.

`trackAdImpression` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

You will want to set these arguments programmatically, across all of your ad zones/slots. For guidelines on how to achieve this with the [OpenX adserver](http://www.openx.com/publisher/enterprise-ad-server), please see the following section [3.6.4](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#ad-example).

### **trackAdClick**

Ad click tracking is accomplished using the `trackAdClick` method. Here are the arguments it accepts:

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `targetUrl` | Yes | The destination URL | string |
| `clickId` | No | Identifier for the particular click instance | string |
| `costModel` | No | The cost model for the campaign: 'cpc', 'cpm', or 'cpa' | string |
| `cost` | No | Ad cost | number |
| `bannerId` | No | Adserver identifier for the ad banner \(creative\) being displayed | string |
| `zoneId` | No | Adserver identifier for the zone where the ad banner is located | string |
| `advertiserID` | No | Adserver identifier for the advertiser which the campaign belongs to | string |
| `campaignId` | No | Adserver identifier for the ad campaign which the banner belongs to | string |

An example:

```javascript
snowplow_name_here('trackAdClick:' + rnd,

    'http://www.example.com',  // targetUrl
    '12243253',                // clickId
    'cpm',                     // costModel
     2.5,                      // cost
    '23',                      // bannerId
    '7',                       // zoneId
    '67965967893',             // impressionId - the same as in trackAdImpression
    '201',                     // advertiserId
    '12'                       // campaignId
);
```

Ad click events are implemented as Snowplow unstructured events.[Here](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/ad_click/jsonschema/1-0-0) is the JSON schema for an ad click event.

`trackAdClick` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

### **trackAdConversion**

Use the `trackAdConversion` method to track ad conversions. Here are the arguments it accepts:

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `conversionId` | No | Identifier for the particular conversion instance | string |
| `costModel` | No | The cost model for the campaign: 'cpc', 'cpm', or 'cpa' | string |
| `cost` | No | Ad cost | number |
| `category` | No | Conversion category | number |
| `action` | No | The type of user interaction, e.g. 'purchase' | string |
| `property` | No | Describes the object of the conversion | string |
| `initialValue` | No | How much the conversion is initially worth | number |
| `advertiserID` | No | Adserver identifier for the advertiser which the campaign belongs to | string |
| `campaignId` | No | Adserver identifier for the ad campaign which the banner belongs to | string |

An example:

```text
window.snowplow_name_here('trackAdConversion',

    '743560297', // conversionId
    10,          // cost
    'ecommerce', // category
    'purchase',  // action
    '',          // property
    99,          // initialValue - how much the conversion is initially worth
    '201',       // advertiserId
    'cpa',       // costModel
    '12'         // campaignId
);
```

Ad conversion events are implemented as Snowplow unstructured events. [Here](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/ad_conversion/jsonschema/1-0-0) is the schema for an ad conversion event.

`trackAdConversion` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

### **Granary and OpenX**

Most ad servers enable you to append custom code to your ad tags. Here's what the zone append functionality looks like in the OpenX adserver \(OnRamp edition\):

![](../../../.gitbook/assets/openx.png)

 You will need to populate the ad zone append field with Snowplow tags for **every ad zone/unit**which you use to serve ads across your site or network. Read on for the Snowplow HTML code to use for OpenX.

### **OpenX: Magic Macros**

Because OpenX has a feature called [magic macros](http://www.openx.com/docs/whitepapers/magic-macros), it is relatively straightforward to pass the banner, campaign and user ID arguments into the call to `trackAdImpression()` \(advertiser ID is not available through magic macros\).

The full HTML code to append, using asynchronous Snowplow invocation, looks like this:

```markup
<!-- Snowplow starts plowing -->
<script type="text/javascript">
;(function(p,l,o,w,i,n,g){if(!p[i]){p.GlobalSnowplowNamespace=p.GlobalSnowplowNamespace||[];
p.GlobalSnowplowNamespace.push(i);p[i]=function(){(p[i].q=p[i].q||[]).push(arguments)
};p[i].q=p[i].q||[];n=l.createElement(o);g=l.getElementsByTagName(o)[0];n.async=1;
n.src=w;g.parentNode.insertBefore(n,g)}}(window,document,"script","//d1fc8wv8zag5ca.cloudfront.net/2.5.1/sp.js","snowplow_name_here"));

// Update tracker constructor to use your CloudFront distribution subdomain
window.snowplow_name_here('newTracker', 'cf', 'patldfvsg0d8w.cloudfront.net');

window.snowplow_name_here('trackAdImpression', '{impressionId}' '{costModel}', '{cost}', '{targetUrl}', '{bannerid}', '{zoneid}', '{advertiserId}', '{campaignid}'); // OpenX magic macros. Leave this line as-is

 </script>
<!-- Snowplow stops plowing -->
```

Once you have appended this code to all of your active ad zones, Snowplow should be collecting all of your ad impression data.

## Custom Structured Events

There are likely to be a large number of AJAX events that can occur on your site, for which a specific tracking method is part of Snowplow. Examples include:

* Playing a video
* Adding an item to basket
* Submitting a lead form

Our philosophy in creating Snowplow is that users should capture "every" consumer interaction and work out later how to use this data. This is different from traditional web analytics and business intelligence, that argues that you should first work out what you need, and only then start capturing the data.

As part of a Snowplow implementation, therefore, we recommend that you identify every type of AJAX interaction that a user might have with your site: each one of these is an event that will not be captured as part of the standard page view tracking. All of them are candidates to track using `trackStructEvent`, if none of the other event-specific methods outlined above are appropriate.

### **trackStructEvent**

There are five parameters can be associated with each structured event. Of them, only the first two are required:

| **Name** | **Required?** | **Description** |
| :--- | :--- | :--- |
| `Category` | Yes | The name you supply for the group of objects you want to track e.g. 'media', 'ecomm' |
| `Action` | Yes | A string which defines the type of user interaction for the web object e.g. 'play-video', 'add-to-basket' |
| `Label` | No | An optional string which identifies the specific object being actioned e.g. ID of the video being played, or the SKU or the product added-to-basket |
| `Property` | No | An optional string describing the object or the action performed on it. This might be the quantity of an item added to basket |
| `Value` | No | An optional float to quantify or further describe the user action. This might be the price of an item added-to-basket, or the starting time of the video where play was just pressed |

The async specification for the `trackStructEvent` method is:

```javascript
snowplow_name_here('trackStructEvent', 'category','action','label','property','value');
```

An example of tracking a user listening to a music mix:

```javascript
snowplow_name_here('trackStructEvent', 'Mixes', 'Play', 'MrC/fabric-0503-mix', '', '0.0');
```

Note that in the above example no value is set for the `event property`.

`trackStructEvent` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

## Unstructured events

You may wish to track events on your website or application which are not directly supported by Snowplow and which [structured event tracking](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-structured-events) does not adequately capture. Your event may have more than the five fields offered by `trackStructEvent`, or its fields may not fit into the category-action-label-property-value model. The solution is Snowplow's self-describing events \(previously known as unstructured\). Self-describing events use JSONs which can have arbitrarily many fields.

To define your own custom event, you must create a [JSON schema](http://json-schema.org/) for that event and upload it to an [Iglu Schema Repository](https://github.com/snowplow/iglu). Snowplow uses the schema to validate that the JSON containing the event properties is well-formed.

### **trackSelfDescribingEvent**

To track a self-describing event, you make use the `trackSelfDescribingEvent` method:

```javascript
snowplow_name_here('trackSelfDescribingEvent', <<SELF-DESCRIBING EVENT JSON>>);
```

For example:

```javascript
window.snowplow_name_here('trackSelfDescribingEvent', {
    schema: 'iglu:com.acme_company/viewed_product/jsonschema/2-0-0',
    data: {
        productId: 'ASO01043',
        category: 'Dresses',
        brand: 'ACME',
        returning: true,
        price: 49.95,
        sizes: ['xs', 's', 'l', 'xl', 'xxl'],
        availableSince: new Date(2013,3,7)
    }
});
```

The second argument is a [self-describing JSON](http://snowplowanalytics.com/blog/2014/05/15/introducing-self-describing-jsons/). It has two fields:

* A `data` field, containing the properties of the event
* A `schema` field, containing the location of the [JSON schema](http://json-schema.org/) against which the `data` field should be validated.

The `data` field should be flat, not nested.

`trackSelfDescribingEvent` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

## Link click tracking

Link click tracking is enabled using the `enableLinkClickTracking` method. Use this method once and the Tracker will add click event listeners to all link elements. Link clicks are tracked as unstructured events. Each link click event captures the link's href attribute. The event also has fields for the link's id, classes, and target \(where the linked document is opened, such as a new tab or new window\).

[Here](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/link_click/jsonschema/1-0-1) is the JSON schema for a link click event.

### **enableLinkClickTracking**

Turn on link click tracking like this:

```javascript
snowplow_name_here('enableLinkClickTracking');
```

This is its signature:

```javascript
enableLinkClickTracking(filter, pseudoclicks, content);
```

You can control which links are tracked using the second argument. There are three ways to do this: a blacklist, a whitelist, and a filter function.

#### **Blacklists**

This is an array of CSS classes which should be ignored by link click tracking. For example, the below code will stop link click events firing for links with the class "barred" or "untracked", but will fire link click events for all other links:

```javascript
snowplow_name_here('enableLinkClickTracking', {'blacklist': ['barred', 'untracked']});
```

If there is only one class name you wish to blacklist, you don't need to put it in an array:

```javascript
snowplow_name_here('enableLinkClickTracking', {'blacklist': 'barred'});
```

#### **Whitelists**

The opposite of a blacklist. This is an array of the CSS classes of links which you do want to be tracked. Only clicks on links with a class in the list will be tracked.

```javascript
snowplow_name_here('enableLinkClickTracking', {'whitelist': ['unbarred', 'tracked']});
```

If there is only one class name you wish to whitelist, you don't need to put it in an array:

```javascript
snowplow_name_here('enableLinkClickTracking', {'whitelist': 'unbarred'});
```

#### **Filter functions**

You can provide a filter function which determines which links should be tracked. The function should take one argument, the link element, and return either 'true' \(in which case clicks on the link will be tracked\) or 'false' \(in which case they won't\).

The following code will track clicks on those and only those links whose id contains the string "interesting":

```javascript
function myFilter (linkElement) {
  return linkElement.id.indexOf('interesting') > -1;
}

snowplow_name_here('enableLinkClickTracking', {'filter': myFilter});
```

The second optional parameter is `pseudoClicks`. If this is not turned on, Firefox will not recognise middle clicks. If it is turned on, there is a small possibility of false positives \(click events firing when they shouldn't\). Turning this feature on is recommended:

```javascript
snowplow_name_here('enableLinkClickTracking', null, true);
```

The third optional parameter is `content`. Set it to `true` if you want link click events to capture the innerHTML of the clicked link:

```javascript
snowplow_name_here('enableLinkClickTracking', null, null, true);
```

The innerHTML of a link is all the text between the `a` tags. Note that if you use a base 64 encoded image as a link, the entire base 64 string will be included in the event.

Each link click event will include \(if available\) the destination URL, id, classes and target of the clicked link. \(The target attribute of a link specifies a window or frame where the linked document will be loaded.\)

`enableLinkClickTracking` can also be passed an array of custom contexts to attach to every link click event as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

### **refreshLinkClickTracking**

`enableLinkClickTracking` only tracks clicks on links which exist when the page has loaded. If new links can be added to the page after then which you wish to track, just use `refreshLinkClickTracking`. This will add Snowplow click listeners to all links which do not already have them \(and which match the blacklist, whitelist, or filter function you specified when `enableLinkClickTracking` was originally called\). Use it like this:

```javascript
snowplow_name_here('refreshLinkClickTracking');
```

### **trackLinkClick**

You can manually track individual link click events with the `trackLinkClick` method. This is its signature:

```javascript
function trackLinkClick(targetUrl, elementId, elementClasses, elementTarget, elementContent);
```

Of these arguments, only `targetUrl` is required. This is how to use `trackLinkClick`:

```javascript
snowplow_name_here('trackLinkClick', 'http://www.example.com', 'first-link', ['class-1', 'class-2'], '', 'this page');
```

`trackLinkClick` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

## Form tracking

Snowplow automatic form tracking detects two event types:

#### **change\_form**

When a user changes the value of a `textarea`, `input`, or `select` element inside a form, a [`change_form`](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/change_form/jsonschema/1-0-0) event will be fired. It will capture the name, type, and new value of the element, and the id of the parent form.

#### **submit\_form**

When a user submits a form, a [`submit_form`](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/submit_form/jsonschema/1-0-0) event will be fired. It will capture the id and classes of the form and the name, type, and value of all `textarea`, `input`, and `select` elements inside the form.

Note that this will only work if the original form submission event is actually fired. If you prevent it from firing, for example by using a jQuery event handler which returns `false` to handle clicks on the form's submission button, the Snowplow `submit_form` event will not be fired.

### **enableFormTracking**

Use the `enableFormTracking` method to add event listeners to turn on form tracking by adding event listeners to all form elements and to all interactive elements inside forms \(that is, all `input`, `textarea`, and `select` elements\).

```javascript
snowplow('enableFormTracking');
```

This will only work for form elements which exist when it is called. If you are creating a form programatically, call `enableFormTracking` again after adding it to the document to track it. \(You can call `enableFormTracking` multiple times without risk of duplicated events.\)

Note that events on password fields will not be tracked.

### **Custom form tracking**

It may be that you do not want to track every field in a form, or every form on a page. You can customize form tracking by passing a configuration argument to the `enableFormTracking` method. This argument should be an object with two elements named "forms" and "fields". The "forms" element determines which forms will be tracked; the "fields" element determines which fields inside the tracked forms will be tracked. As with link click tracking, there are three ways to configure each field: a blacklist, a whitelist, or a filter function. You do not have to use the same method for both fields.

#### **Blacklists**

This is an array of strings used to prevent certain elements from being tracked. Any form with a CSS class in the array will be ignored. Any field whose name property is in the array will be ignored. All other elements will be tracked.

#### **Whitelists**

This is an array of strings used to turn on certail. Any form with a CSS class in the array will be tracked. Any field in a tracked form whose name property is in the array will be tracked. All other elements will be ignored.

#### **Filter functions**

This is a function used to determine which elements are tracked. The element is passed as the argument to the function and is tracked if and only if the value returned by the function is truthy.

#### **Transform functions**

This is a function used to transform data in each form field. The element is passed as the argument to the function and is replaced by the value returned.

**Examples**

To track every form element and every field except those fields named "password":

```javascript
var config = {
  forms: {
    blacklist: []
  },
  fields: {
    blacklist: ['password']
  }
};

snowplow('enableFormTracking', config);
```

To track only the forms with CSS class "tracked", and only those fields whose ID is not "private":

```javascript
var config = {
  forms: {
    whitelist: ["tracked"]
  },
  fields: {
    filter: function (elt) {
      return elt.id !== "private";
    }
  }
};

snowplow('enableFormTracking', config);
```

To transform the form fields with an MD5 hashing function:

```javascript
var config = {
  forms: {
    whitelist: ["tracked"]
  },
  fields: {
    filter: function (elt) {
      return elt.id !== "private";
    },
    transform: function (elt) {
      return MD5(elt);
    }
  }
};

snowplow('enableFormTracking', config);
```

## `trackAddToCart` and `trackRemoveFromCart`

These methods let you track users adding and removing items from a cart on an ecommerce site. Their arguments are identical:

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `sku` | Yes | Item SKU | string |
| `name` | No | Item name | string |
| `category` | No | Item category | string |
| `unitPrice` | Yes | Item price | number |
| `quantity` | Yes | Quantity added to or removed from cart | number |
| `currency` | No | Item price currency | string |

An example:

```javascript
window.snowplow_name_here('trackAddToCart', '000345', 'blue tie', 'clothing', 3.49, 2, 'GBP');
window.snowplow_name_here('trackRemoveFromCart', '000345', 'blue tie', 'clothing', 3.49, 1, 'GBP');
```

Both methods are implemented as Snowplow unstructured events. You can see schemas for the [`add_to_cart`](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/add_to_cart/jsonschema/1-0-0) and [`remove_from_cart`](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/remove_from_cart/jsonschema/1-0-0) events.

Both methods can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

## `trackSiteSearch`

Use the `trackSiteSearch` method to track users searching your website. Here are its arguments:

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `terms` | Yes | Search terms | array |
| `filters` | No | Search filters | JSON |
| `totalResults` | No | Results found | number |
| `pageResults` | No | Results displayed on first page | number |

An example:

```javascript
window.snowplow_name_here('trackSiteSearch',
    ['unified', 'log'],      // search terms
    {'category': 'books', 'sub-category': 'non-fiction'},  // filters
    14,                      // results found
    8                        // results displayed on first page
);
```

Site search events are implemented as Snowplow unstructured events. [Here](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/site_search/jsonschema/1-0-0) is the schema for a `site_search` event.

`trackSiteSearch` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

## `trackTiming`

Use the `trackTiming` method to track user timing events such as how long resources take to load. Here are its arguments:

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `category` | Yes | Timing category | string |
| `variable` | Yes | Timed variable | string |
| `timing` | Yes | Number of milliseconds elapsed | number |
| `label` | No | Label for the event | string |

An example:

```javascript
window.snowplow('trackTiming',
  'load',            // Category of the timing variable
  'map_loaded',      // Variable being recorded
  50,                // Milliseconds taken
  'Map loading time' // Optional label
);
```

Site search events are implemented as Snowplow unstructured events. [Here](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow/timing/jsonschema/1-0-0) is the schema for a `timing` event.

`trackTiming` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

## Consent tracking

### **tackConsentGranted**

Use the `trackConsentGranted` method to track a user opting into data collection. A consent document context will be attached to the event if at least the `id` and `version` arguments are supplied. The method arguments are:

| **Name** | **Description** | **Required?** | **Type** |
| :--- | :--- | :--- | :--- |
| `id` | Identifier for the document granting consent | Yes | String |
| `version` | Version of the document granting consent | Yes | String |
| `name` | Name of the document granting consent | No | String |
| `description` | Description of the document granting consent | No | String |
| `expiry` | Date-time string specifying when consent document expires | No | String |
| `context` | Custom context for the event | No | Array |
| `tstamp` | When the event occurred | No | Positive integer |

The `expiry` field specifies that the user consents to the attached documents until the date-time provided, after which the consent is no longer valid.

Tracking a consent granted event:

```javascript
window.snowplow('trackConsentGranted',
  '1234',                        // Id
  '5',                           // Version
  'consent_document',            // Name
  'a document granting consent', // Description
  '2020-11-21T08:00:00.000Z'     // Expiry
);
```

### **trackConsentWithdrawn**

Use the `trackConsentWithdrawn` method to track a user withdrawing consent for data collection. A consent document context will be attached to the event if at least the `id` and `version` arguments are supplied. To specify that a user opts out of all data collection, `all` should be set to `true`.

The method arguments are:

| **Name** | **Description** | **Required?** | **Type** |
| :--- | :--- | :--- | :--- |
| `all` | Specifies whether all consent should be withdrawn | No | Boolean |
| `id` | Identifier for the document withdrawing consent | No | String |
| `version` | Version of the document withdrawing consent | No | string |
| `name` | Name of the document withdrawing consent | No | String |
| `description` | Description of the document withdrawing consent | No | String |
| `context` | Custom context for the event | No | Array |
| `tstamp` | When the event occurred | No | Positive integer |

Tracking a consent withdrawn event:

```javascript
window.snowplow('trackConsentWithdrawn',
  false,                            // All
  '1234',                           // Id
  '5',                              // Version
  'consent_document',               // Name
  'a document withdrawing consent'  // Description
);
```

### **Consent documents**

Consent documents are stored in the context of a consent event. Each consent method adds a consent document to the event. The consent document is a custom context storing the arguments supplied to the method \(in both granted and withdrawn events, this will be: id, version, name, and description\). In either consent method, additional documents can be appended to the event by passing an array of consent document self-describing JSONs in the context argument.

The fields of a consent document are:

| **Name** | **Description** | **Required?** | **Type** |
| :--- | :--- | :--- | :--- |
| `id` | Identifier for the document | Yes | String |
| `version` | Version of the document | Yes | String |
| `name` | Name of the document | No | String |
| `description` | Description of the document | No | String |

A consent document self-describing JSON looks like this:

```javascript
{
  schema: 'iglu:com.snowplowanalytics.snowplow/consent_document/jsonschema/1-0-0',
  data: {
    id: '1234',
    version: '5',
    name: 'consent_document_name',
    description: 'here is a description'
  }
}
```

As an example, `trackConsentGranted` will store one consent document as a custom context:

```javascript
window.snowplow('trackConsentGranted',
  '1234',                        // Id
  '5',                           // Version
  'consent_document',            // Name
  'a document granting consent', // Description
  '2020-11-21T08:00:00.000Z'     // Expiry
);
```

The method call will generate this event:

```javascript
{
  e: 'ue',
  ue_pr: {
    schema: 'iglu:com.snowplowanalytics.snowplow/unstruct_event/jsonschema/1-0-0',
    data: {
      schema: 'iglu:com.snowplowanalytics.snowplow/consent_granted/jsonschema/1-0-0',
      data: {
        expiry: '2020-11-21T08:00:00.000Z'
      }
    }
  },
  co: {
    schema: 'iglu:com.snowplowanalytics.snowplow/contexts/jsonschema/1-0-0',
    data: {
      schema: 'iglu:com.snowplowanalytics.snowplow/consent_document/jsonschema/1-0-0',
      data: {
        id: '1234',
        version: '5',
        name: 'consent_document',
        description: 'a document granting consent'
      }
    }
  }
}
```

## Custom contexts

Custom contexts can be used to augment any standard Snowplow event type, including unstructured events, with additional data.

Custom contexts can be added as an extra argument to any of Snowplow's `track..()` methods and to `addItem` and `addTrans`.

Each custom context is a self-describing JSON following the same pattern as an [unstructured event](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#trackUnstructEvent). As with unstructured events, if you want to create your own custom context, you must create a [JSON schema](http://json-schema.org/) for it and upload it to an [Iglu repository](https://github.com/snowplow/iglu). Since more than one \(of either different or the same type\) can be attached to an event, the `context` argument \(if it is provided at all\) should be a non-empty array of self-describing JSONs.

**Important:** Even if only one custom context is being attached to an event, it still needs to be wrapped in an array.

Here are two example custom context JSONs. One describes a page, and the other describes a user on that page.

```javascript
{
    schema: "iglu:com.example_company/page/jsonschema/1-2-1",
    data: {
        pageType: 'test',
        lastUpdated: new Date(2014,1,26)
    }
}
```

```javascript
{
    schema: "iglu:com.example_company/user/jsonschema/2-0-0",
    data: {
      userType: 'tester'
    }
}
```

How to track a page view with both these contexts attached:

```javascript
window.snowplow_name_here('trackPageView', null, [{
    schema: "iglu:com.example_company/page/jsonschema/1-2-1",
    data: {
        pageType: 'test',
        lastUpdated: new Date(2014,1,26)
    }
},
{
    schema: "iglu:com.example_company/user/jsonschema/2-0-0",
    data: {
      userType: 'tester'
    }
}]);
```

In this case an empty string has been provided to the optional `customTitle` argument in order to reach the `context` argument.

For more information on custom contexts, see [this](http://snowplowanalytics.com/blog/2014/01/27/snowplow-custom-contexts-guide/) blog post.

## **Error tracking**

_Note that this is only available on version 2.7.0 and above of the JS tracker._

Snowplow JS tracker provides two ways of tracking exceptions: manual tracking of handled exceptions using `trackError` and automatic tracking of unhandled exceptions using `enableErrorTracking`.

### **trackError**

Use the `trackError` method to track handled exceptions \(application errors\) in your JS code. This is its signature:

```javascript
function (message, filename, lineno, colno, error, contexts)
```

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `message` | Yes | Error message | string |
| `filename` | No | Filename or URL | string |
| `lineno` | No | Line number of problem code chunk | number |
| `colno` | No | Column number of problem code chunk | number |
| `error` | No | JS `ErrorEvent` | ErrorEvent |

Of these arguments, only `message` is required. Signature of this method defined to match `window.onerror` callback in modern browsers.

```javascript
try {
  var user = getUser()
} catch(e) {
  snowplow_name_here('trackError', 'Cannot get user object', 'shop.js', null, null, e);
}
```

`trackError` can also be passed an array of custom contexts as an additional final parameter. See [Contexts](https://github.com/snowplow/snowplow/wiki/2-Specific-event-tracking-with-the-Javascript-tracker#custom-contexts) for more information.

Using `trackError` it's assumed that developer knows where error could happen, which is not often the case. Therefor it's recommended to use `enableErrorTracking` as it allows you to discover errors that weren't expected.

### **enableErrorTracking**

Use the `enableErrorTracking` method to track unhandled exceptions \(application errors\) in your JS code. This is its signature:

```javascript
function (filter, contextAdder)
```

| **Name** | **Required?** | **Description** | **Type** |
| :--- | :--- | :--- | :--- |
| `filter` | No | Predicate function to filter exceptions | Function ErrorEvent =&gt; Boolean |
| `contextAdder` | No | Function to grab custom contexts | Function ErrorEvent =&gt; Array |

Unlike `trackError` you need enable error tracking only once:

```javascript
snowplow_name_here('enableErrorTracking')
```

Application error events are implemented as Snowplow unstructured events. [Here](https://raw.githubusercontent.com/snowplow/iglu-central/master/schemas/com.snowplowanalytics.snowplow/application_error/jsonschema/1-0-1) is the schema for a `application_error` event.

## Setting the true timestamp

As standard, every event tracked by the Javascript tracker will be recorded with two timestamps:

1. A `device_created_tstamp` - set when the event occurred
2. A `device_sent_tstamp` - set when the event was sent by the tracker to the collector

These are combined downstream in the Snowplow pipeline \(with the `collector_tstamp`\) to calculate the `derived_tstamp`, which is our best estimate of when the event actually occurred.

In certain circumstances you might want to set the timestamp yourself e.g. if the JS tracker is being used to process historical event data, rather than tracking the events live. In this case you can set the `true_timestamp` for the event. When set, this will be used as the value in the `derived_tstamp`rather than a combination of the `device_created_tstamp`, `device_sent_tstamp` and `collector_tstamp`.

To set the true timestamp add an extra argument to your track method:

```javascript
{type: 'ttm', value: unixtimestamp}
```

e.g. to set a true timestamp with a page view event:

```javascript
window.snowplow_name_here('trackPageView', null, [{
    schema: "iglu:com.example_company/page/jsonschema/1-2-1",
    data: {
        pageType: 'test',
        lastUpdated: new Date(2014,1,26)
    }
},
{
    schema: "iglu:com.example_company/user/jsonschema/2-0-0",
    data: {
      userType: 'tester'
    }
}],
{type: 'ttm', value: 1482511723});
```

e.g. to set a true timestamp for a self-describing event:

```javascript
window.snowplow_name_here('trackSelfDescribingEvent', {
    schema: 'iglu:com.acme_company/viewed_product/jsonschema/2-0-0',
    data: {
        productId: 'ASO01043',
        category: 'Dresses',
        brand: 'ACME',
        returning: true,
        price: 49.95,
        sizes: ['xs', 's', 'l', 'xl', 'xxl'],
        availableSince: new Date(2013,3,7)
    }
}, null, {type: 'ttm', value: 1482511723});
```

