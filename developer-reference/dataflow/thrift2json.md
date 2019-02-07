---
description: 'https://gitlab.alvary.io/grnry/grnry-extractor'
---

# Metadata Extractor



![](../../.gitbook/assets/thrift2json.png)

A Python App that extracts a Correlation ID and a Timestamp to be used for creation and update of profiles as well as keys for queries against the eventstore.

## Configuration

| Name | Type | Default Value | Description |
| :--- | :--- | :--- | :--- |
|  |  |  |  |

#### Input Format

The Metadata Extractor can be configured to consume either Thrift or JSON payload. By default, it expects a Thrift payload.

#### Correlation ID Rules

In order to find the right ID in each record, rules as to where to find these IDs in the payload should be specified in a YAML file. For more details, refer to below guidelines and example.

{% tabs %}
{% tab title="Spec" %}
A yaml file is expected that contains a set of labeled regular expressions which are all applied to the input message. For matching rules the specified part is extracted and passed into the outgoing message using the rule-label as the name of the forwarded attribute.

The ID extraction rules are defined as follows in the id-extraction-rules.yaml:

* First hierarchy layer is a source categorization \(currently unused\)
* The second hierarchy layer is used to distinguish between different event types \(currently unused\).
* The third hierarchy defines the rules to get the correlation ID the format: -&lt;json path separated by "/"&gt; : &lt;regex&gt;

Currently the regex is not used.

Example:

```text
web:
    snowplow_js:
        - body/data/0/duid: ""
    facebook:
        - url: "https://facebook.com/(.*?)/"
```

In the example above a check for a JSON element such as this example 

```text
{"body": {"data":[{"duid":"abc"}]} }
```

would be firstly executed and the resulting correlation ID would have the value of 

```text
body/data/0/duid__abc
```

If the first check fails, a check for a JSON element like

```text
{"url":"https://www.google.com"}
```

would be performed and the resulting correlation ID would be 

```text
url__https://www.google.com
```

{% hint style="info" %}
IMPORTANT!

* The first rule that finds an element wins
* "\_\_" is forbidden in JSON paths
* Numbers in JSON paths are only allowed to represent integer keys or array indices, not string number keys
{% endhint %}
{% endtab %}

{% tab title="Example" %}
{% code-tabs %}
{% code-tabs-item title=" id-extraction-rules.yaml " %}
```yaml
web:
  snowplow_js:
    - body/data/0/duid: ""
  snowplow_redirect:
    - body/se_va: ""
  facebook:
    - body/url: "https://facebook.com/(.*?)/"
  userid:
    - body/uid: ".*?"
  allianz:
    - body/vertragspartner: ".*?"
    - body/riskhash: ".*?"
mobile:
  snowplow_android:
    - body/data/0/uid: ""
  appid:
    - body/aid: ".*?"
  imei:
    - body/deviceid: ".*"
  linkid:
    - body/se_va: '[-+]?[0-9]*\.?[0-9]*'
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Input Topic `'raw'`

{% tabs %}
{% tab title="Spec" %}
Thrift payload with Kafka consumer record fields. The payload will be saved under "value" field and contains the following [fields.](https://gitlab.alvary.io/grnry/grnry-extractor/blob/master/grnry/grnryextractor/res/collector-payload.thrift)

| Metadata Fields | Description |
| :--- | :--- |
| key | uuid created by Snowplow Kafka Producer |
| value | Payload written by Snowplow |
| $$-$$ schema | Snowplow Event Schema Reference |
| $$-$$ ipAddress | ipAddress if Snowplow is configured to collect this |
| $$-$$ timestamp  | time of event creation or reception\(?\) |
| $$-$$ collector  | identifies the source platform of the event |
| $$-$$ body  | Snowplow Event Data \(depending on `schema`\) |
| $$-$$ headers  | HTTP headers |
| $$-$$ ...  | ... |
|  |  |
{% endtab %}

{% tab title="Example" %}
```text
d
                                                          10.42.3.107
�hLB�:
      �UTF-8
            �ssc-0.14.0-kafka
                             ,xMozilla/5.0 (Macintosh; Intel Mac OS X 10_14_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
                                                                                                                                                       66http://grnry.pages.alvary.io/demo-app-sprung-tracking/
                                                                                                                                                                                                               @#/com.snowplowanalytics.snowplow/tp2
         T�{"schema":"iglu:com.snowplowanalytics.snowplow/payload_data/jsonschema/1-0-4","data":[{"e":"pv","url":"http://grnry.pages.alvary.io/demo-app-sprung-tracking/","page":"Alvary Snowplow Download Test","tv":"js-2.9.0","tna":"AlvarySnowplow","aid":"AlvarySPWeb2AppTest","p":"web","tz":"Europe/Berlin","lang":"de-DE","cs":"UTF-8","f_pdf":"1","f_qt":"0","f_realp":"0","f_wma":"0","f_dir":"0","f_fla":"0","f_java":"0","f_gears":"0","f_ag":"0","res":"3440x1440","cd":"24","cookie":"1","eid":"520e75e5-2f97-4749-b3dc-928bae878c40","dtm":"1547467657277","vp":"1684x1115","ds":"1669x1459","vid":"6","sid":"79c030bb-f0af-43f6-9a8f-b6694dc63ae3","duid":"12abe3a9-2aad-47eb-822d-7f019f54dadd","fp":"1918457671","stm":"1547467657281"}]}^
                           Host: development.lce.grnry.io$Origin: http://grnry.pages.alvary.io�User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
                                                                                                                                                                                                                                   Accept: */*?Referer: http://grnry.pages.alvary.io/demo-app-sprung-tracking/"Accept-Encoding: gzip, deflate, br7Accept-Language: de-DE, de;q=0.9, en-US;q=0.8, en;q=0.7~Cookie: play-basic-authentication-filter=2cbf913c03bb3f2fe86810cf7ae9595c67500b0d; JSESSIONID=A92B89668196AB35593E0CF9064F9D5Cx-forwarded-proto: https2x-request-id: ce00fde2-b017-45db-b095-b3f6b44b1445$x-envoy-expected-rq-timeout-ms: 3000>x-envoy-original-path: /api/com.snowplowanalytics.snowplow/tp2imeout-Access: <function1>application/json
                                happlication/json
                                                 �development.lce.grnry.io
                                                                          �$c4e78535-d522-4948-b10e-50bd33b8f9e7
                                                                                                                ziAiglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0
```
{% endtab %}
{% endtabs %}

## Output Topic `'raw-json'`

{% tabs %}
{% tab title="Spec" %}
<table>
  <thead>
    <tr>
      <th style="text-align:left">Payload Fields</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">metadata</td>
      <td style="text-align:left">Kafka specific fields</td>
    </tr>
    <tr>
      <td style="text-align:left">payload</td>
      <td style="text-align:left">Forwarded from input attribute <code>value </code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em></em> schema</td>
      <td style="text-align:left">Snowplow Event Schema Reference</td>
    </tr>
    <tr>
      <td style="text-align:left"><em></em> ipAddress</td>
      <td style="text-align:left">ipAddress if Snowplow is configured to collect this</td>
    </tr>
    <tr>
      <td style="text-align:left"><em></em> timestamp</td>
      <td style="text-align:left">time of event creation or reception(?)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em></em> collector</td>
      <td style="text-align:left">identifies the source platform of the event</td>
    </tr>
    <tr>
      <td style="text-align:left"><em></em> body</td>
      <td style="text-align:left">Snowplow Event Data (depending on <code>schema</code>)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em></em> headers</td>
      <td style="text-align:left">HTTP headers</td>
    </tr>
    <tr>
      <td style="text-align:left">payloadid</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">correlationid</td>
      <td style="text-align:left">
        <p>extracted from <code>payload.body</code> 
        </p>
        <p>Used to group events received from the same tracking entity.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">eventid</td>
      <td style="text-align:left">used to deduplicate events</td>
    </tr>
    <tr>
      <td style="text-align:left">created</td>
      <td style="text-align:left">used to recreate the original order of events</td>
    </tr>
  </tbody>
</table>
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



