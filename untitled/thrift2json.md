---
description: 'https://gitlab.alvary.io/grnry/thrift-to-json'
---

# thrift2json

![](../.gitbook/assets/dataflow_thrift2json.png)

A Python App that extracts a Correlation ID and a Timestamp to be used for creation and update of profiles as well as keys for queries against the eventstore.

## Input

{% tabs %}
{% tab title="Spec" %}
| Metadata Fields | Payload Fields | Description |
| :--- | :--- | :--- |
| key |  | uuid created by Snowplow Kafka Producer |
| value |  | Payload written by Snowplow |
|  | _schema_ | Snowplow Event Schema Reference |
|  | _ipAddress_ | ipAddress if Snowplow is configured to collect this |
|  | _timestamp_ | time of event creation or reception\(?\) |
|  | _collector_ | identifies the source platform of the event |
|  | _body_ | Snowplow Event Data \(depending on `schema`\) |
|  | _headers_ | HTTP headers |
|  | ... | ... |
{% endtab %}

{% tab title="Example" %}

{% endtab %}
{% endtabs %}

## Configuration

{% tabs %}
{% tab title="Spec" %}
A yaml file is expected that contains a set of labeled regular expressions which are all applied to the `value.body` attribute of the input message. For matching rules the specified part is extracted and passed into the outgoing message using the rule-label as the name of the forwarded attribute.
{% endtab %}

{% tab title="Example" %}
{% code-tabs %}
{% code-tabs-item title=" id-extraction-rules.yaml " %}
```yaml
web:
  snowplow:
    - id: ".*?"
  facebook:
    - url: "https://facebook.com/(.*?)/"
  userid:
    - uid: ".*?"
  allianz:
    - vertragspartner: ".*?"
    - riskhash: ".*?"
mobile:
  appid:
    - aid: ".*?"
  imei:
    - deviceid: ".*"
  linkid:
    - se_va: '[-+]?[0-9]*\.?[0-9]*'
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Output

{% tabs %}
{% tab title="Spec" %}
| Metadata Fields | Payload Fields | Description |
| :--- | :--- | :--- |
| metadata |  | Kafka specific fields |
| payload |  | Forwarded from input attribute `value`  |
|  | _schema_ | Snowplow Event Schema Reference |
|  | _ipAddress_ | ipAddress if Snowplow is configured to collect this |
|  | _timestamp_ | time of event creation or reception\(?\) |
|  | _collector_ | identifies the source platform of the event |
|  | _body_ | Snowplow Event Data \(depending on `schema`\) |
|  | _headers_ | HTTP headers |
| payloadid |  |  |
|  | correlationid | extracted from `payload.body`. Used to group events received from the same tracking entity. |
|  | eventid | used to deduplicate events |
|  | created | used to recreate the original order of events |
{% endtab %}

{% tab title="Example" %}
```javascript
{
	"metadata": "{\"headers\": null, \"topic\": \"raw\", \"partition\": 0, \"key\": \"9417415d-7359-4ad0-8c3b-46ff4ca78c44\", \"timestamp\": [1, 1540748298485], \"offset\": 1746229}",
	"payload": {
		"body": "{ ... }",
		"collector": "ssc-0.13.0-kafka",
		"encoding": "UTF-8",
		"headers": ["Host: aws-eu1.grnry.io", "Accept: text/html, application/xhtml+xml, application/xml;q=0.9, image/webp, image/apng, */*;q=0.8", "Accept-Encoding: gzip, deflate, br", "Accept-Language: de-DE, de;q=0.9, en-US;q=0.8, en;q=0.7", "Cookie: _ga=GA1.2.1323636424.1533625971; ajs_anonymous_id=%22058bbd8d-cb74-47e4-aa27-9cc5fa4546aa%22; ajs_group_id=null; ajs_user_id=%22QjAVZpXa7qgJdZs6vGsNMA5M9yH3%22; mp_96b84420a1a32e448f73e7b9ffccebdb_mixpanel=%7B%22distinct_id%22%3A%20%22165133b330da0-0ad91354656274-47e1039-1fa400-165133b330f15e%22%2C%22%24initial_referrer%22%3A%20%22%24direct%22%2C%22%24initial_referring_domain%22%3A%20%22%24direct%22%7D", "Upgrade-Insecure-Requests: 1", "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36", "X-Forwarded-For: 82.207.192.113", "X-Forwarded-Port: 443", "X-Forwarded-Proto: https", "Connection: keep-alive", "Timeout-Access: <function1>"],
		"hostname": "aws-eu1.grnry.io",
		"ipAddress": "82.207.192.113",
		"networkUserId": "631b9979-16b8-44ed-87b1-86b7d92d8223",
		"path": "/i",
		"schema": "iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0",
		"timestamp": 1535972952029,
		"userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36"
	},
    {
      "payloadid" : {
         "correlationid": "cookie_dW5kIG5pZW1hbHMgdmVyZ2Vzc2VuIGVpc2VybiB1bmlvbg==",
         "eventid": "819f785b-82f7-4994-bbd8-992c94bdf7bc",
         "created": 1541345852941,
      }
    }
}
```
{% endtab %}
{% endtabs %}

