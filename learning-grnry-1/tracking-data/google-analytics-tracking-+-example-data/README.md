# Google Analytics Tracking + Example Data

### IDs provided by GA

| ID | Description |
| :--- | :--- |
| Client ID | Random UUID, provided via a first party cookie, 2 year lifetime |
| User ID  | ID set by Business, if set Client ID is not used, used after Login, must not be PII itself |
| jid | Join ID, used to link Google Cookie to a DoubleClick cookie |
| gjid | ??? |
| \_v | Adsense link ID, private |
| \_gid | google cookie id with 24h lifetime, private |

### Sample Event parameters 

| Parameter | Example Value | Explanation |
| :--- | :--- | :--- |
| v | 1 | Protocol Version \(is used to provide information about backwards compatibility, will likely stay 1\) |
| \_v | j68 | SDK version Number, actual Version number of the Trackersoftware |
| t | pageview | Type, possible values: pageview, screenview, event, transaction, item, social, exception, timing; gives information about further parameters included in the event log |
| \_s | 1 | sequence counter \(in Session or Cookie life time?\) |
| dl | https%3A%2F%2Fphoenix-dummy.herokuapp.com%2Fchoice\_a | document location url, urlencoded |
| ul | de | user language |
| de | utf-8 | document encoding |
| dt | Auswahl%20A | document title, url encoded |
| sd  | 24-bit | color depth |
| sr | 1536x864 | screen resolution |
| vp | 1536x750 | viewport size |
| je   | 0  | Java enabled \(boolean\) |
| cid | 2054910945.1533217168 | Client Id, see above |
| tid | UA-122481378-1 | Tracking ID, identifiying the tracking party |
| \_gid | 1098838428.1533543622 | analytics.js cookie with 24h lifetime |
| \_u | SCCAAEAD~ | Verification code of GA |
| a | 364452476 | Adsense 2 GA link |
| z | 1153956582 | cachec buster, do not use, except for dedoublication |

### Further Documentation

{% embed url="https://developers.google.com/analytics/devguides/collection/protocol/v1/" %}

{% embed url="https://discourse.snowplowanalytics.com/t/sending-google-analytics-events-into-snowplow/1201" %}

{% embed url="https://www.cheatography.com/dmpg-tom/cheat-sheets/google-universal-analytics-url-collect-parameters/" %}



