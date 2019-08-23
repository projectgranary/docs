# Load and Setup

## Loading Snowplow.js

Use the following tag to your page to load Snowplow.js:

```markup
<script type="text/javascript" async=1>
;(function(p,l,o,w,i,n,g){if(!p[i]){p.GlobalSnowplowNamespace=p.GlobalSnowplowNamespace||[];
p.GlobalSnowplowNamespace.push(i);p[i]=function(){(p[i].q=p[i].q||[]).push(arguments)
};p[i].q=p[i].q||[];n=l.createElement(o);g=l.getElementsByTagName(o)[0];n.async=1;
n.src=w;g.parentNode.insertBefore(n,g)}}(window,document,"script","//api.grnry.io/sp-2.9.0.js","snowplow_name_here"));
</script>
```

_Important note regarding testing:_ `"//api.grnry.io/sp-2.9.0.js"` is the protocol-relative URL used to fetch `sp.js`. It will work if the your web page is using the "http" or "https" protocol. But if you are testing locally and loading your page from your filesystem using the "file" protocol \(so its URI looks something like "file:///home/joe/snowplow\_test.html"\), the protocol-relative URL will also use that protocol, preventing the script from loading. To avoid this, change the URL to `"http://api.grnry.io/sp-2.9.0.js"` when testing locally.

As well as loading Snowplow, this tag creates a global function called "snowplow\_name\_here" which you use to access the Tracker. You can replace the string "snowplow\_name\_here" with the function name of your choice. This is encouraged: if there are two Snowplow users on the same page, there won't be any conflict between them as long as they have chosen different function names. The rest of the documentation will assume that the function is called "snowplow\_name\_here".

Once the function `snowplow_name_here` is created, the syntax for using Snowplow methods is as follows:

```javascript
snowplow_name_here({{"methodName"}}, {{first method argument}}, {{second method argument}}, ...);
```

For example, the method `trackStructEvent` has this signature:

```javascript
function trackStructEvent(category, action, label, property, value, context)
```

where only the first two arguments are required. You would use it like this:

```javascript
snowplow_name_here('trackStructEvent', 'Mixes', 'Play', '', '', 20);
```

Empty strings are provided for the `label` and `property` arguments to pad them out. \(`Null`would also work.\) They won't be added to the event. Neither will the context argument, which isn't provided at all.

## Initialising a tracker

Tracker initialization is indicated with the `"newTracker"` string and takes three arguments:

1. The tracker namespace
2. The collector endpoint
3. An optional argmap containing other settings

Here is a simple example of how to initialise a tracker:

```javascript
snowplow_name_here("newTracker", "cf", "api.grnry.io", {
  appId: "cfe23a",
  platform: "mob"
});
```

The tracker will be named "cf" and will send events to d3rkrsqld9gmqf.cloudfront.net, the cloudfront collector specified. The final argument is called the argmap. Here it is just used to set the app ID and platform for the tracker. Each event the tracker sends will have an app ID field set to "cfe23a" and a platform field set to "mob".

Here is a longer example in which every tracker configuration parameter is set:

```javascript
snowplow_name_here("newTracker", "cf", "api.grnry.io", {
  appId: "cfe23a",
  platform: "mob"
  cookieDomain: null,
  discoverRootDomain: true,
  cookieName: "_sp534_",
  encodeBase64: false,
  respectDoNotTrack: false,
  userFingerprint: true,
  userFingerprintSeed: 6385926734,
  pageUnloadTimer: 0,
  forceSecureTracker: true,
  post: true,
  bufferSize: 5,
  maxPostBytes: 45000,
  crossDomainLinker: function (linkElement) {
    return (linkElement.href === "http://acme.de" || linkElement.id === "crossDomainLink");
  },
  cookieLifetime: 86400 * 31,
  stateStorageStrategy: "cookie",
  contexts: {
    webPage: true,
    performanceTiming: true,
    gaCookies: true,
    geolocation: false
  }
});
```

We will now go through the various argmap parameters. Note that these are all optional. In fact, you aren't required to provide any argmap at all.

### **Setting the application ID**

Set the application ID using the `appId` field of the argmap. This will be attached to every event the tracker fires. You can set different application IDs on different parts of your site. You can then distinguish events that occur on different applications by grouping results based on `application_id`.

### **Setting the platform**

Set the application platform using the `platform` field of the argmap. This will be attached to every event the tracker fires. Its default value is "web". For a list of supported platforms, please see the [Snowplow Tracker Protocol](https://github.com/snowplow/snowplow/wiki/SnowPlow-Tracker-Protocol).

### **Configuring the cookie domain**

If your website spans multiple subdomains e.g.

* [www.mysite.com](http://www.mysite.com/)
* blog.mysite.com
* application.mysite.com

You will want to track user behaviour across all those subdomains, rather than within each individually. As a result, it is important that the domain for your first party cookies is set to '.mysite.com' rather than 'www.mysite.com'. By doing so, any values that are stored on the cookie on one of subdomain will be accessible on all the others.

It is recommended that you [enable automatic discovery and setting of the root domain](https://github.com/snowplow/snowplow/wiki/1-General-parameters-for-the-Javascript-tracker#discoverRootDomain).

Otherwise, set the cookie domain for the tracker instance using the `cookieDomain` field of the argmap. If this field is not set, the cookies will not be given a domain.

**WARNING**: _Changing the cookie domain will reset all existing cookies. As a result, it might be a major one-time disruption to data analytics because all visitors to the website will receive a new `domain_userid`._

### **Configuring the cookie name**

Set the cookie name for the tracker instance using the `cookieName` field of the argmap. The default is "_sp_". Snowplow uses two cookies, a domain cookie and a session cookie. In the default case, their names are "\_sp\_id" and "\_sp\_ses" respectively. If you are upgrading from an earlier version of Snowplow, you should use the default cookie name so that the cookies set by the earlier version are still remembered. Otherwise you should provide a new name to prevent clashes with other Snowplow users on the same page.

Once set, you can retrieve a cookie name thanks to the `getCookieName(basename)` method where basename is id or ses for the domain and session cookie respectively. As an example, you can retrieve the complete name of the domain cookie with `getCookieName('id')`.

### **Configuring base 64 encoding**

By default, self-describing events and custom contexts are encoded into Base64 to ensure that no data is lost or corrupted. You can turn encoding on or off using the `encodeBase64` field of the argmap.

### **Respecting Do Not Track**

Most browsers have a Do Not Track option which allows users to express a preference not to be tracked. You can respect that preference by setting the `respectDoNotTrack` field of the argmap to `true`. This prevents cookies from being sent and events from being fired.

### **Opt-out cookie**

It is possible to set an opt-out cookie in order not to track anything similarly to Do Not Track through `setOptOutCookie(cookieName)`. If this cookie is set, cookies won't be stored and events won't be fired.

**2.2.8 User fingerprinting**

By default, the tracker generates a user fingerprint based on various browser features. This fingerprint is likely to be unique and so can be used to track anonymous users. You can turn user fingerprinting off by setting the `userFingerprint` field of the argmap to `false`.

### **Setting the user fingerprint seed**

The `userFingerprintSeed` field of the the argmap lets you choose the hash seed used to generate the user fingerprint. If this is not specified, the default is 123412414.

### **Setting the page unload pause**

Whenever the Snowplow Javascript Tracker fires an event, it automatically starts a 500 millisecond timer running. If the user clicks on a link or refreshes the page during this period \(or, more likely, if the event was triggered by the user clicking a link\), the page will wait until either the event is sent or the timer is finished before unloading. 500 milliseconds is usually enough to ensure the event has time to be sent.

You can change the pause length \(in milliseconds\) using the `pageUnloadTimer` of the argmap. The above example completely eliminates the pause. This does make it unlikely that events triggered by link clicks will be sent.

See also [How the Tracker uses `localStorage`](https://github.com/snowplow/snowplow/wiki/1-General-parameters-for-the-Javascript-tracker#local-storage) for an explanation of how the tracker can later recover and send unsent events.

### **Setting the event request protocol**

Normally the protocol \(http or https\) used by the Tracker to send events to a collector is the same as the protocol of the current page. You can force it to use https by setting the `forceSecureTracker` field of the argmap to `true`.

### **Setting an unsecure event request protocol**

Normally the protocol \(http or https\) used by the Tracker to send events to a collector is the same as the protocol of the current page. You can force it to use http by setting the `forceUnsecureTracker` field of the argmap to `true`. If `forceSecureTracker` is activated this argument is ignored.

**NOTE**: This argument should only be used for testing purposes as it creates security vulnerabilities.

### **Configuring the session cookie duration**

Whenever an event fires, the Tracker creates a session cookie. If the cookie didn't previously exist, the Tracker interprets this as the start of a new session.

By default the session cookie expires after 30 minutes. This means that a user leaving the site and returning in under 30 minutes does not change the session. You can override this default by setting `sessionCookieTimeout` to a duration \(in seconds\) in the argmap. For example,

```text
{
  ...
  sessionCookieTimeout: 3600
  ...
}
```

would set the session cookie lifespan to an hour.

### **Configuring the storage strategy**

Three strategies are made available to store the Tracker's state: cookies, local storage or no storage at all. You can set the strategy with the help of the `stateStorageStrategy` parameter in the argmap to "cookie" \(the default\), "localStorage" or "none" respectively.

When choosing local storage, the Tracker will additionally store events in local storage before sending them so that they can be recovered if the user leaves the page before they are sent.

### **Adding predefined contexts**

The JavaScript Tracker comes with many predefined contexts which you can automatically add to every event you send. To enable them, simply add them to the `contexts` field of the argmap as above.



