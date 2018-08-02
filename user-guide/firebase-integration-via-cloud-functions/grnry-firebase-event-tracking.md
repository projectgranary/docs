# GRNRY Firebase Event tracking

As the amount of Events to be labeled as conversions is limited \(and therefore the amount of event types to be forewareded to snowplow\) we would advise that if you plan on tracking many different custom events to consolidate them into one GRNRY\_CUSTOM1 event with up to 25 custom keys \(consistent over all events of this type !\). This might however result in less optimal performance of the forewareding cloud function !. See below for an example:

```java
Bundle grnryBundle = new Bundle();
grnryBundle = initBundle(grnryBundle);
grnryBundle.putString("GRNRY_CUSTOM_PARAMETER_1", "Value 1");
grnryBundle.putString("GRNRY_CUSTOM_PARAMETER_2", "Value 2");
grnryBundle.putString("GRNRY_CUSTOM_PARAMETER_3", "Value 3");
FirebaseAnalytics.getInstance(getApplicationContext()).logEvent("GRNRY_CUSTOM1", grnryBundle);
```

