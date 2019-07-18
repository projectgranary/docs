---
description: 'On this page, you get the details about the Metadata Extractor'
---

# Metadata Extractor

The Metadata Extractor is used to extract header information from the message. Therefore four different expressions need to be filled with information based on the Spring Expression Language.

* grnry-correlation-id
* grnry-event-id
* grnry-event-type
* grnry-harvester-name

**Name**: grnry-data-in-metadata

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

In addition, the required fields, which you need to fill with Spring Expression Language expressions and their associated type are:

```yaml
metadata.correlationIdExpression: SpEL-Expression
metadata.eventIdExpression: SpEL-Expression
metadata.timestampExpression: SpEL-Expression
metadata.harvesterName: Literal
metadata.eventTypeName: Literal
```

One sample of filling this information is:

```yaml
app.grnry-data-in-metadata.metadata.correlationIdExpression: "#safeJsonPath(payload, 'kd_nr')?:'NO_CORRELATION_ID'",
app.grnry-data-in-metadata.metadata.eventIdExpression: "#randomUUID()",
app.grnry-data-in-metadata.metadata.eventTypeName: "${grnry.eventTypeName}",
app.grnry-data-in-metadata.metadata.harvesterName: "${grnry.harvesterName}",
app.grnry-data-in-metadata.metadata.timestampExpression: "#nowMillis()",
app.grnry-data-in-metadata.grnry.harvesterName: "sftp-all-std-harvester",
app.grnry-data-in-metadata.grnry.eventTypeName: "sftp-all"
  
```

Please note, that in this sample, the part `app.grnry-data-in-metadata` is the general stream definition.

**Source Parameters:**

The current documentation for the Spring Expression Language \(SpEL\) can be found here:

{% embed url="https://docs.spring.io/spring/docs/5.1.8.RELEASE/spring-framework-reference/core.html\#expressions-language-ref" caption="Documentation on the Spring Expression Language" %}

### 

