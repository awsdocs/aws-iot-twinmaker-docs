--------

--------

# Example component types<a name="twinmaker-component-types-examples"></a>

This topic contains examples that show how to implement key concepts of component types\.

## Alarm \(abstract\)<a name="twinmaker-component-types-examples-alarm"></a>

The following example is the abstract alarm component type that appears in the AWS IoT TwinMaker console\. It contains a `functions` list that consists of a `dataReader` that has no `implementedBy` value\.

```
{
  "componentTypeId": "com.example.alarm.basic:1",
  "workspaceId": "MyWorkspace",
  "description": "Abstract alarm component type",
  "functions": {
    "dataReader": {
         "isInherited": false
    }
  },
  "isSingleton": false,
  "propertyDefinitions": {
    "alarm_key": {
      "dataType": {
        "type": "STRING"
      },
      "isExternalId": true,
      "isRequiredInEntity": true,
      "isStoredExternally": false,
      "isTimeSeries": false
    },
    "alarm_status": {
      "dataType": {
        "allowedValues": [
          {
            "stringValue": "ACTIVE"
          },
          {
            "stringValue": "SNOOZE_DISABLED"
          },
          {
            "stringValue": "ACKNOWLEDGED"
          },
          {
            "stringValue": "NORMAL"
          }
        ],
        "type": "STRING"
      },
      "isRequiredInEntity": false,
      "isStoredExternally": true,
      "isTimeSeries": true
    }
  }
}
```

Notes:

Values for `componentTypeId` and `workspaceID` are required\. The value of `componentTypeId` must be unique to your workspace\. The value of `alarm_key` is a unique identifier that a function can use to retrieve alarm data from an external source\. The value of the key is required and stored in AWS IoT TwinMaker\. The `alarm_status` time series values are stored in the external source\.

More examples are available in [AWS IoT TwinMaker Samples](https://github.com/aws-samples/aws-iot-twinmaker-samples)\.

## Timestream telemetry<a name="twinmaker-component-types-examples-telemetry"></a>

The following example is a simple component type that retrieves telemetry data about a specific type of asset \(such as an alarm or a cookie mixer\) from an external source\. It specifies a Lambda function that component types inherit\.

```
{
    "componentTypeId": "com.example.timestream-telemetry",
    "workspaceId": "MyWorkspace",
    "functions": {
        "dataReader": {
            "implementedBy": {
                "lambda": {
                    "arn": "lambdaArn"
                }
            }
        }
    },
    "propertyDefinitions": {
        "telemetryAssetType": {
            "dataType": {
                "type": "STRING"
            },
            "isExternalId": false,
            "isStoredExternally": false,
            "isTimeSeries": false,
            "isRequiredInEntity": true
        },
        "telemetryAssetId": {
            "dataType": {
                "type": "STRING"
            },
            "isExternalId": false,
            "isStoredExternally": false,
            "isTimeSeries": false,
            "isRequiredInEntity": true
        }
    }
}
```

## Alarm \(inherits from abstract alarm\)<a name="twinmaker-component-types-examples-alarm-implementation"></a>

The following example inherits from both the abstract alarm and the timestream telemetry component types\. It specifies its own Lambda function that retrieves alarm data\.

```
{
    "componentTypeId": "com.example.cookiefactory.alarm",
    "workspaceId": "MyWorkspace",
    "extendsFrom": [
        "com.example.timestream-telemetry",
        "com.amazon.iottwinmaker.alarm.basic"
    ],
    "propertyDefinitions": {
        "telemetryAssetType": {
            "defaultValue": {
                "stringValue": "Alarm"
            }
        }
    },
    "functions": {
        "dataReader": {
            "implementedBy": {
                "lambda": {
                    "arn": "lambdaArn"
                }
            }
        }
    }
}
```

**Note**  
Because the alarm connector inherits from the abstract alarm component type, the Lambda function must return the `alarm_key` value\. If you don't return this value, Grafana won't recognize it as an alarm\. This is required for all components that return alarms\.

## Equipment examples<a name="twinmaker-component-types-examples-equipment"></a>

The examples in this section show how to model potential pieces of equipment\. You can use these examples to get some ideas about how to model equipment in your own processes\.

### Cookie mixer<a name="twinmaker-component-types-examples-mixer"></a>

The following example inherits from the timestream telemetry component type\. It specifies additional time\-series properties for a cookie mixer's rotation rate and temperature\.

```
{
    "componentTypeId": "com.example.cookiefactory.mixer",
    "workspaceId": "MyWorkspace",
    "extendsFrom": [
        "com.example.timestream-telemetry"
    ],
    "propertyDefinitions": {
        "telemetryAssetType": {
            "defaultValue" : { "stringValue": "Mixer" }
        },
        "RPM": {
            "dataType": { "type": "DOUBLE" },
            "isTimeSeries": true,
            "isStoredExternally": true
        },
        "Temperature": {
            "dataType": { "type": "DOUBLE" },
            "isTimeSeries": true,
            "isStoredExternally": true
        }
    }
}
```

### Water tank<a name="twinmaker-component-types-examples-watertank"></a>

The following example inherits from the timestream telemetry component type\. It specifies additional time\-series properties for a water tank's volume and flow rate\.

```
{
    "componentTypeId": "com.example.cookiefactory.watertank",
    "workspaceId": "MyWorkspace",
    "extendsFrom": [
        "com.example.timestream-telemetry"
    ],
    "propertyDefinitions": {
        "telemetryAssetType": {
            "defaultValue" : { "stringValue": "WaterTank" }
        },
        "tankVolume1": {
            "dataType": { "type": "DOUBLE" },
            "isTimeSeries": true,
            "isStoredExternally": true
        },
        "tankVolume2": {
            "dataType": { "type": "DOUBLE" },
            "isTimeSeries": true,
            "isStoredExternally": true
        },
        "flowRate1": {
            "dataType": { "type": "DOUBLE" },
            "isTimeSeries": true,
            "isStoredExternally": true
        },
        "flowrate2": {
            "dataType": { "type": "DOUBLE" },
            "isTimeSeries": true,
            "isStoredExternally": true
        }
    }
}
```

### Space location<a name="twinmaker-component-types-examples-space"></a>

The following example contains properties, the values of which are stored in AWS IoT TwinMaker\. Because the values are specified by users and stored internally, no function is required to retrieve them\. The example also uses the `RELATIONSHIP` data type to specify a relationship with another component type\.

This component provides a lightweight mechanism for adding context to a digital twin\. You can use it to add metadata indicating where something is located\. You can also use this information in logic used for determining which cameras can see a piece of equipment or space, or for knowing how to dispatch someone to a location\.

```
{
    "componentTypeId": "com.example.cookiefactory.space",
    "workspaceId": "MyWorkspace",
    "propertyDefinitions": {
        "position":  {"dataType": {"nestedType": {"type": "DOUBLE"},"type": "LIST"}},
        "rotation":  {"dataType": {"nestedType": {"type": "DOUBLE"},"type": "LIST"}},
        "bounds":  {"dataType": {"nestedType": {"type": "DOUBLE"},"type": "LIST"}},
        "parent_space" : { "dataType": {"type": "RELATIONSHIP"}}
    }
}
```