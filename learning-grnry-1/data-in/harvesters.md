# Harvesters

## Prerequisites

Before you can create a harvester, you need to define an [event type](event-types.md) and also [register a source type implementation](source-types.md) that matches the type of your data source \(s3 / jdbc / webservice\).

## harvester definition

todo

## Creating a harvester

To create a new harvester you have to set at least the following fields:

#### displayName

This is what the the harvester instance will be called. This name needs to be unique and it will derive a unique technical name of the harvester instance from this field.

#### sourceType

You need to provide at least the name and the version of the desired source type.

#### eventType

You need to provide at least the name and the version of the event type, which you want the the harvester instance to process.

The remaining fields are optional and will be filled with default values if there are none provided.

TODO: optional fields?

#### Example Call

```text
curl -X POST -H "Content-Type: application/json" \
  -d @create-harvester.json \
  https://grnry-host/harvesters/instances
  
create-harvester.json content:

{
  "displayName": "harvester-1",
  "sourceType": {
    "name": "source-type-1",
    "version": "latest"
  },
  "eventType": {
    "name": "event-type-1",
    "version": "latest"
  }
}
```

## updating a harvester

To update an existing harvester instance you can make a PUT API Call.

#### Example Call

```text
curl -X PUT -H "Content-Type: application/json" \
  -d @update-harvester.json \
  https://grnry-host/harvesters/instances/harvester-1
  
update-harvester.json content:

{
  "description": "new description"
}
```

This call updates the `description` of the previous created harvester.

## starting and stopping a harvester

In order to see the current state of a harvester instance you can do:

```text
curl -X GET -H "Content-Type: application/json" \
  https://grnry-host/harvesters/instances/harvester-name/state
  
RESPONSE:
{
  "status": "STOPPED",
  "_links": {
    "_self": {
      "href": "https://grnry-host/harvesters/instances/harvester-name/state"
    }
  }
}
```

To start/stop the harvester instance you need to send a POST Api Call to `/harvesters/instances/:harvester-name/state`.

#### Example Call

```text
curl -X GET -H "Content-Type: application/json" \
  -d @harvester-state.json \
  https://grnry-host/harvesters/instances/harvester-name/state

harvester-state.json content:

{
  "action": "START"
}
```

**Note:** To stop a harvester instance the action field in `harvester-state.json` needs to be `STOP`

#### Different states of a Harvester Instance

A harvester instance can be in one of the following states:

| State | Description |
| :--- | :--- |
| **RUNNING** | The instance is currently running and operational. |
| **DEPLOYING** | The instance is currently deployed. |
| **RUNNING\_BUT\_OUTDATED** | The instance is currently running but there is a newer version available. |
| **FAILED** | The instance failed to deploy. |
| **STOPPING** | The instance is currently being stopped. |
| **STOPPED** | The instance has stopped. |
| **UNKNOWN** | The state of the instance is unknown. |

