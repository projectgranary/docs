# Snowplow Web \(&App\) Tracking + Example Data

### IDs

| ID | Explanation |
| :--- | :--- |
| User ID | set by business |
| domain\_userid  | Userid set by first party cookie, lifetime? |
| network\_userid | Userid set by third party cookie, lifetime? |
| user ip adress | IP or Mac Adress, privacy issue? |
| fingerprint | browser fingerprint, not unique, how stable? |

### Web Example Data

| Parameter | example value | explanation |
| :--- | :--- | :--- |
| e | pv | eventtype - PageView |
| url | [https://phoenix-dummy.herokuapp.com/](https://phoenix-dummy.herokuapp.com/) | Page URL |
| page | Startseite | Page Title |
| tv | js-2.6.2 | tracker version |
| tna | ssc | tracker namespace |
| aid | snowplow-tracker-demo-v1.0 | appication id  \(unique for website or App\) |
| p | web | platform |
| tz | Europe/Berlin | operating system timezone |
| lang | de | Language of Browser |
| cs | UTF-8 | webpage encoding |
| res | 1536x864 | screen resolution |
| cd | 24 | browser color depth |
| cookie | 1 | cookies permitted |
| eid | b873c2e3-36e9-485a-a4c3-022d3ae43218 | event id |
| dtm | 1533648645784 | timestamp when event was recorded by client |
| vp | 1536x750 | viewport size |
| ds | 1536x750 | webpage size |
| vid | 1 | domain session index |
| sid | fbca39fb-c526-435a-9c97-44a078a70be5 | domain session id |
| duid | effb0446-6ab3-468c-902e-a471bf563ca3 | Domain User Id, first party cookie |
| fp | 3384011938 | browser fingerprint |
| stm | 1533648652862 | timestamp when event was sent \(not created!\) |

### App Tracking Example values

repeats from web are not shown

| Parameter | Example Value |  |
| :--- | :--- | :--- |
| cx | bWFpbHRvOmZsb3JpYW4uZW5nZWxrZUBhbHZhcnkuaW8/c3ViamVjdD1pJTIwZ290JTIweW91JTIx | App Context, base 64 encoded |
| ue\_px | bWFpbHRvOmZsb3JpYW4uZW5nZWxrZUBhbHZhcnkuaW8/c3ViamVjdD1SZWFsbHklM0Y= | unstructured event content, base 64 encoded  |
| ip | F080::7000:9EDF:FB06:9994 | ip or mac adress |
|  |  |  |



### Documentation Links

{% embed url="https://github.com/snowplow/snowplow/wiki/snowplow-tracker-protocol" %}

