# Firebase User Properties

To hand over informations specific to the app installation with every Firebase Analytics Event it is possible to define Firebase user properties.

```java
FirebaseAnalytics.getInstance(getApplicationContext()).setUserProperty("KEY","VALUE");
```

We would advise you to use two properties for GRNRY specific, first a GRNRUID which should be random UUID and second a GRNRYSESSIONID \(also a random UUID\) that is reset whenever a new session \(specifics to be defined\) is started.

