# Snowplow API endpoints

Analog to the [Snowplow trackers](https://github.com/snowplow/snowplow/wiki/trackers), the Snowplow API accepts both GET and POST requests and is located at **`/api/com.snowplowanalytics.snowplow/tp2`** . Upon receipt, the API **could** set/update a third-party cookie to allow cross-domains user tracking.  Then the ID in this third-party user tracking cookie would be stored in the `network_userid` field in Snowplow events. To activate this, set "crossDomain" and/or "cookie" fields to ENABLED in the collector [configuration file](https://gitlab.alvary.io/grnry/deployment/blob/master/charts/incubator/snowplow-scala-stream-collector/templates/configmap.yaml). Currently, they are both **disabled**.

GET requests to the API should have the event's parameters appended to the end of each request, while POST requests should include them in the request body. The selection of event parameters is arbitrary. For a list of common parameters used by Snowplow tracker, please refer [here](https://github.com/snowplow/snowplow/wiki/snowplow-tracker-protocol).

{% api-method method="get" host="https://api.grnry.io" path="/api/com.snowplowanalytics.snowplow/tp2?param1=test&param2=test..." %}
{% api-method-summary %}
Tracker endpoint GET
{% endapi-method-summary %}

{% api-method-description %}
Event parameters should be appended to the end of each request.Example:`https://api.grnry.io/api/com.snowplowanalytics.snowplow/tp2?f_java=1&aid=lcePrices&tv=js-0.26.1&e=pv&ds=1105x390&cookie=1`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="user-agent" type="string" required=false %}
Agent firing this request
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/api/com.snowplowanalytics.snowplow/tp2" %}
{% api-method-summary %}
Tracker endpoint POST
{% endapi-method-summary %}

{% api-method-description %}
Event parameters should be added into the body as String.Example JSON body:{"id" : "1234", "tsa": "test"}
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="user-agent" type="string" required=false %}
Agent firing this request
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

