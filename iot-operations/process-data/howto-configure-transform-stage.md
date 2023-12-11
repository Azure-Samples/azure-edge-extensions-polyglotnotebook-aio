---
title: Use jq to transform data in a pipeline
description: Configure a transform pipeline stage to configure a data transformation with jq in a Data Processor pipeline.
author: dominicbetts
ms.author: dobett
ms.subservice: data-processor
ms.topic: how-to
ms.custom:
  - ignite-2023
ms.date: 10/09/2023

#CustomerIntent: As an operator, I want to transform data in a pipeline so that I can make structural transformations messages.
---

# Transform data in a pipeline

[!INCLUDE [public-preview-note](../includes/public-preview-note.md)]

Use the _transform_ stage to carry out structural transformations on messages in a pipeline, such as:

- Rename tags and properties
- Unbatch data
- Add new properties
- Add calculated values

The transform stage uses [jq](concept-jq.md) to support data transformation:

- Each pipeline partition transforms messages independently of each other.
- The stage outputs a transformed message based on the [jq expression](concept-jq-expression.md) you provide.
- Create a [jq expression](concept-jq-expression.md) to transform a message based on the structure of the incoming message to the stage.  

## Prerequisites

To configure and use a transform pipeline stage, you need:

- A deployed instance of Azure IoT Data Processor (preview).
- An understanding of [jq expressions](concept-jq-expression.md).

### Configure the stage

The transform stage JSON configuration defines the details of the stage. To author the stage, you can either interact with the form-based UI or provide the JSON configuration on the **Advanced** tab:

| Name | Value | Required | Example | 
| --- | --- | --- | --- |
| Name  | A name to show in the Data Processor UI.  | Yes | `Transform1` |
| Description | A user-friendly description of what the transform stage does.  | No | `Rename Tags` |
| Query | The transformation [jq expression](concept-jq-expression.md).  | Yes | `.payload.values |= (map({(.tag): (.numVal // .boolVal)}) | add)` |

## Sample configuration

The following transformation example converts the array of tags in the input message into an object that contains all the tags and their values:

```json
{
    "displayName": "TransformInput", 
    "description": "Make array of tags into one object", 
    "query": ".payload.values |= (map({(.tag): (.numVal // .boolVal)}) | add)"
}
```

The output from the transform stage looks like the following example:

```json
{
  "systemProperties": {
    "partitionKey": "foo",
    "partitionId": 5,
    "timestamp": "2023-01-11T10:02:07Z"
  },
  "qos": 1,
  "topic": "/assets/foo/tags/bar",
  "properties": {
    "responseTopic": "outputs/foo/tags/bar",
    "contentType": "application/json",
    "payloadFormat": 1,
    "correlationData": "base64::Zm9v",
    "messageExpiry": 412
  },
  "userProperties": [
    {
      "key": "prop1",
      "value": "value1"
    },
    {
      "key": "prop2",
      "value": "value2"
    }
  ],
  "payload": {
    "values": {
      "temperature": 250,
      "pressure": 30,
      "humidity": 10,
      "runningStatus": true
    }
  }
}
```

## Related content

- [Aggregate data in a pipeline](howto-configure-aggregate-stage.md)
- [Enrich data in a pipeline](howto-configure-enrich-stage.md)
- [Filter data in a pipeline](howto-configure-filter-stage.md)
- [Call out to a gRPC endpoint from a pipeline](howto-configure-grpc-callout-stage.md)
- [Call out to an HTTP endpoint from a pipeline](howto-configure-http-callout-stage.md)
- [Use last known values in a pipeline](howto-configure-lkv-stage.md)
