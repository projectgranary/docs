---
description: >-
  This page provides a guide for adding a retry mechanism to your Snowplow Java
  client application in case of Granary's Snowplow tracking endpoint is
  unavailable.
---

# Retry Mechanism for Snowplow Clients

## Option 1: ClosableHttpClient

As mentioned in the docu about setting up the [ApacheHTTPClientAdapter](https://github.com/snowplow/snowplow/wiki/Java-Tracker#512-apachehttpclientadapter), it is possible to configure a `ClosableHttpClient` to pass into the `HttpClientAdapter`. 

Using the `HttpClientBuilder` to instantiate the `ClosableHttpClient` we can configure a retry handler using:

```java
setRetryHandler(new DefaultHttpRequestRetryHandler(NUM_CONNECTION_RETRIES, true));
```

This allows quick connection retries for any of the following Exceptions occurring client-side: `InterruptedIOException`, `UnknownHostException`, `ConnectException`, `SSLException`. 

## Option 2: EmitterCallback

The second way to go which also allows more advanced/custom logic of handling the retries is adding an [EmitterCallback](https://github.com/snowplow/snowplow/wiki/Java-Tracker#55-emitter-callback) where a `RequestCallback` is passed in, which resends events in the `onFailure()`-method.

A sample of how both strategies could be used together should look similar to this:

```java
// Make a new client with custom concurrency rules
PoolingHttpClientConnectionManager manager = new PoolingHttpClientConnectionManager();
manager.setDefaultMaxPerRoute(50);

// Use the client builder to allow 10 direct connection retries from the HttpClient
HttpClientBuilder builder = HttpClients
       .custom()
       .setConnectionManager(manager);
       .setRetryHandler(new DefaultHttpRequestRetryHandler(10, true));
CloseableHttpClient httpClient = builder.build();

// Build the adapter
HttpClientAdapter adapter = ApacheHttpClientAdapter.builder()
      .url("https://www.acme.com")
      .httpClient(client)
      .build();


// Configure your custom callback for advanced handling of failure to send events
RequestCallback callback = new RequestCallback() {
  @Override
  public void onSuccess(int successCount) {
    System.out.println("Success, successCount: " + successCount);
  }

  @Override
  public void onFailure(int successCount, List<TrackerPayload> failedEvents) {
    System.out.println("Failure, successCount: " + successCount + "\nfailedEvent:\n" + failedEvents.toString());

    // Simple example logic here, we just directly try to re-send each event but we could also add timeouts here etc.
    for (TrackerPayload payload : failedEvents) {
      tracker.tracker(payload);
    }
  }
};


// Attach everything to an Emitter
Emitter e1 = BatchEmitter.builder()
        .httpClientAdapter(adapter) //set the adapter we configured above
        .requestCallback(callback) //set the requestcallback we defined above
        .build();


//Set the emitter on your tracker
tracker.setEmitter(e1);

```

{% hint style="warning" %}
This proposed solution will block your application until all retries are carried out or Granary's Snowplow endpoint becomes available again. If you need a non-blocking solution, you need to implement a concurrently running thread.
{% endhint %}

