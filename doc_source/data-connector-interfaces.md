--------

--------

# AWS IoT TwinMaker data connectors<a name="data-connector-interfaces"></a>

Connectors need access to your underlying data store to resolve sent queries and to return either results or an error\.

To learn about the available connectors, their request interfaces, and their response interfaces, see the following topics\.

For more information about the properties used in the connector interfaces, see the [GetPropertyValueHistory](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValueHistory.html) API action\.

**Note**  
Some connectors have two timestamp fields in both the request and response interfaces for start time and end time properties\. Both `startDateTime` and `endDateTime` use a long number to represent epoch second, which is no longer supported\. To maintain backwards\-compatibility, we still send a timestamp value to that field, but we recommend using the `startTime` and `endTime` fields that are consistent with our API timestamp format\.

**Topics**
+ [Schema initializer connector](#SchemaInitializer-connector)
+ [DataReaderByEntity](#DataReaderByEntity-connector)
+ [DataReaderByComponentType](#DataReaderByComponentType-connector)
+ [DataReader](#DataReader-connector)
+ [AttributePropertyValueReaderByEntity](#AttributePropertyValueReaderByEntity-connector)
+ [DataWriter](#DataWriter-connector)
+ [Examples](#-connector)

## Schema initializer connector<a name="SchemaInitializer-connector"></a>

The Schema initializer connector is used in the component type or entity lifecycle to fetch the component type or component properties from the underlying data source\. With the schema initializer, properties of the component type or component are automatically imported without explicitly calling an API action to set up properties schemas\.

### SchemaInitializer request interface<a name="SchemaInitializer-request-interface"></a>

```
{
  "workspaceId": "string",
  "entityId": "string",
  "componentName": "string",
  "properties": {
    // property name as key,
    // value is of type PropertyResponse 
    "string": "PropertyResponse"
  }
}
```

**Note**  
The map of properties in this request interface is a PropertyResponse\. For more information, see [PropertyResponse](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_PropertyResponse.html)\.

### SchemaInitializer response interface<a name="SchemaInitializer-response-interface"></a>

```
{
  "properties": {
    // property name as key,
    // value is of type PropertyRequest 
    "string": "PropertyRequest"
  }
}
```

**Note**  
The map of properties in this request interface is a PropertyRequest\. For more information, see [PropertyRequest](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_PropertyRequest.html)\.

## DataReaderByEntity<a name="DataReaderByEntity-connector"></a>

DataReaderByEntity is a data plane connector that's used to get the time\-series values of properties in a single component\.

For information about the property types, syntax, and format of this connector, see the [GetPropertyValueHistory](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValueHistory.html) API action\.

### DataReaderByEntity request interface<a name="DataReaderByEntity-request-interface"></a>

```
{
  "startDateTime": long, // In epoch sec, deprecated
  "startTime": "string", // ISO-8601 timestamp format
  "endDateTime": long, // In epoch sec, deprecated
  "endTime": "string", // ISO-8601 timestamp format
  "properties": { 
    // A map of properties as in the get-entity API response
    // property name as key,
    // value is of type [PropertyResponse](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_PropertyResponse.html) 
    "string": "PropertyResponse"
   }, 
  "workspaceId": "string",
  "selectedProperties": List:"string",
  "propertyFilters": List:PropertyFilter,
  "entityId": "string",
  "componentName": "string",
  "componentTypeId": "string",
  "interpolation": InterpolationParameters,
  "nextToken": "string",
  "maxResults": int,
  "orderByTime": "string"
  }
```

### DataReaderByEntity response interface<a name="DataReaderByEntity-response-interface"></a>

```
{
  "propertyValues": [
    {
      "entityPropertyReference": EntityPropertyReference, // The same as [EntityPropertyReference](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_EntityPropertyReference.html)
      "values": [
        {
        "timestamp": long, // Epoch sec, deprecated
        "time": "string", // ISO-8601 timestamp format
        "value": DataValue // The same as [DataValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DataValue.html)
        }
      ]  
    }
  ],
  "nextToken": "string"
}
```

## DataReaderByComponentType<a name="DataReaderByComponentType-connector"></a>

To get the time\-series values of common properties that come from the same component type, use the data plane connector DataReaderByEntity\. For example, if you define time\-series properties in the component type and you have multiple components using that component type, then you can query those properties across all components in a given a time range\. A common use case for this is when you want to query the alarm status of multiple components for a global view of your entities\.

For information about the property types, syntax, and format of this connector, see the [GetPropertyValueHistory](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValueHistory.html) API action\.

### DataReaderByComponentType request interface<a name="DataReaderByComponentType-request-interface"></a>

```
{
  "startDateTime": long, // In epoch sec, deprecated
  "startTime": "string", // ISO-8601 timestamp format
  "endDateTime": long, // In epoch sec, deprecated
  "endTime": "string", // ISO-8601 timestamp format
  "properties": { // A map of properties as in the get-entity API response
    // property name as key,
    // value is of type [PropertyResponse](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_PropertyResponse.html) 
    "string": "PropertyResponse"
   },  
  "workspaceId": "string",
  "selectedProperties": List:"string",
  "propertyFilters": List:PropertyFilter,
  "componentTypeId": "string",
  "interpolation": InterpolationParameters,
  "nextToken": "string",
  "maxResults": int,
  "orderByTime": "string" 
}
```

### DataReaderByComponentType response interface<a name="DataReaderByComponentType-response-interface"></a>

```
{
  "propertyValues": [
    {
      "entityPropertyReference": EntityPropertyReference, // The same as [EntityPropertyReference](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_EntityPropertyReference.html)
      "values": [
        {
        "timestamp": long, // Epoch sec, deprecated
        "time": "string", // ISO-8601 timestamp format
        "value": DataValue // The same as [DataValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DataValue.html)
        }
      ]  
    }
  ],
  "nextToken": "string"
}
```

## DataReader<a name="DataReader-connector"></a>

DataReader is a data plane connector that can handle both the case of DataReaderByEntity and DataReaderByComponentType\.

For information about the property types, syntax, and format of this connector, see the [GetPropertyValueHistory](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValueHistory.html) API action\.

### DataReader request interface<a name="-request-interface"></a>

 The `EntityId` and `componentName` are optional\.

```
{
  "startDateTime": long, // In epoch sec, deprecated
  "startTime": "string", // ISO-8601 timestamp format
  "endDateTime": long, // In epoch sec, deprecated  
  "endTime": "string", // ISO-8601 timestamp format
  "properties": { // A map of properties as in the get-entity API response
    // property name as key,
    // value is of type PropertyRequest 
    "string": "PropertyRequest"
  },
  
  "workspaceId": "string",
  "selectedProperties": List:"string",
  "propertyFilters": List:PropertyFilter,
  "entityId": "string",
  "componentName": "string",
  "componentTypeId": "string",
  "interpolation": InterpolationParameters,
  "nextToken": "string",
  "maxResults": int,
  "orderByTime": "string"
}
```

### DataReader response interface<a name="DataReader-response-interface"></a>

```
{
  "propertyValues": [
    {
      "entityPropertyReference": EntityPropertyReference, // The same as [EntityPropertyReference](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_EntityPropertyReference.html)
      "values": [
        {
        "timestamp": long, // Epoch sec, deprecated
        "time": "string", // ISO-8601 timestamp format
        "value": DataValue // The same as [DataValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DataValue.html)
        }
      ]  
    }
  ],
  "nextToken": "string"
}
```

## AttributePropertyValueReaderByEntity<a name="AttributePropertyValueReaderByEntity-connector"></a>

AttributePropertyValueReaderByEntity is a data plane connector that you can use to fetch the value of static properties in a single entity\.

For information about the property types, syntax, and format of this connector, see the [ GetPropertyValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValue.html) API action\.

### AttributePropertyValueReaderByEntity request interface<a name="AttributePropertyValueReaderByEntity-request-interface"></a>

```
{
  "properties": {
    // property name as key,
    // value is of type [PropertyResponse](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_PropertyResponse.html) 
    "string": "PropertyResponse"
  }
  
  "workspaceId": "string",
  "entityId": "string",
  "componentName": "string",
  "selectedProperties": List:"string",
}
```

### AttributePropertyValueReaderByEntity response interface<a name="AttributePropertyValueReaderByEntity-response-interface"></a>

```
{
  "propertyValues": {
    "string": { // property name as key
        "propertyReference": EntityPropertyReference, // The same as [EntityPropertyReference](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_EntityPropertyReference.html)
        "propertyValue": DataValue // The same as [DataValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DataValue.html)
    }
}
```

## DataWriter<a name="DataWriter-connector"></a>

DataWriter is a data plane connector that you can use to write time\-series data points back to the underlying data store for properties in a single component\.

For information about the property types, syntax, and format of this connector, see the [BatchPutPropertyValues](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_BatchPutPropertyValues.html) API action\.

### DataWriter request interface<a name="DataWriter-request-interface"></a>

```
{
  "workspaceId": "string",
  "properties": {
    // entity id as key
    "String": {
      // property name as key,
      // value is of type [PropertyResponse](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_PropertyResponse.html)      
      "string": PropertyResponse
    } 
  },
  "entries": [
    {
      "entryId": "string",
      "entityPropertyReference": EntityPropertyReference, // The same as [EntityPropertyReference](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_EntityPropertyReference.html)
      "propertyValues": [
        {
        "timestamp": long, // Epoch sec, deprecated
        "time": "string", // ISO-8601 timestamp format
        "value": DataValue // The same as [DataValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DataValue.html)
        }
      ]
    }
  ]
}
```

### DataWriter response interface<a name="DataWriter-response-interface"></a>

```
{
  "errorEntries": [
    {
      "errors": List:BatchPutPropertyError // The value is a list of type [BatchPutPropertyError](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_BatchPutPropertyError.html)
    } 
  ]
}
```

## Examples<a name="-connector"></a>

The following JSON samples are examples of response and request syntax for multiple connectors\.
+ **SchemaInitializer**:

  The following examples show the schema initializer in a component type lifecycle\.

  **Request**:

  ```
  {
    "workspaceId": "myWorkspace",
    "properties": {
      "modelId": {
        "definition": {
            "dataType": {
                "type": "STRING"
            },
            "isExternalId": true,
            "isFinal": true,
            "isImported": false,
            "isInherited": false,
            "isRequiredInEntity": true,
            "isStoredExternally": false,
            "isTimeSeries": false,
            "defaultValue": {
                "stringValue": "myModelId"
            }
        },
        "value": {
            "stringValue": "myModelId"
        }
      },
      "tableName": {
        "definition": {
            "dataType": {
                "type": "STRING"
            },
            "isExternalId": false,
            "isFinal": false,
            "isImported": false,
            "isInherited": false,
            "isRequiredInEntity": false,
            "isStoredExternally": false,
            "isTimeSeries": false,
            "defaultValue": {
                "stringValue": "myTableName"
            }
        },
        "value": {
            "stringValue": "myTableName"
        }
      }
    }
  }
  ```

  **Response**:

  ```
  {
    "properties": {
      "myProperty1": {
        "definition": {
          "dataType": {
            "type": "DOUBLE",
            "unitOfMeasure": "%"
          },
          "configuration": {
            "myProperty1Id": "idValue"
          },
          "isTimeSeries": true
        }
      },
      "myProperty2": {
        "definition": {
          "dataType": {
            "type": "STRING"
          },
          "isTimeSeries": false,
          "defaultValue": {
            "stringValue": "property2Value"
          }
        }
      }
    }
  }
  ```
+ **Schema initializer in entity lifecycle**:

  **Request**:

  ```
  {
    "workspaceId": "myWorkspace",
    "entityId": "myEntity",
    "componentName": "myComponent",
    "properties": {
      "assetId": {
        "definition": {
            "dataType": {
                "type": "STRING"
            },
            "isExternalId": true,
            "isFinal": true,
            "isImported": false,
            "isInherited": false,
            "isRequiredInEntity": true,
            "isStoredExternally": false,
            "isTimeSeries": false
        },
        "value": {
            "stringValue": "myAssetId"
        }
      },
      "tableName": {
        "definition": {
            "dataType": {
                "type": "STRING"
            },
            "isExternalId": false,
            "isFinal": false,
            "isImported": false,
            "isInherited": false,
            "isRequiredInEntity": false,
            "isStoredExternally": false,
            "isTimeSeries": false
        },
        "value": {
            "stringValue": "myTableName"
        }
      }
    }
  }
  ```

  **Response**:

  ```
  {
    "properties": {
      "myProperty1": {
        "definition": {
          "dataType": {
            "type": "DOUBLE",
            "unitOfMeasure": "%"
          },
          "configuration": {
            "myProperty1Id": "idValue"
          },
          "isTimeSeries": true
        }
      },
      "myProperty2": {
        "definition": {
          "dataType": {
            "type": "STRING"
          },
          "isTimeSeries": false
        },
        "value": {
          "stringValue": "property2Value"
        }
      }
    }
  }
  ```
+ **DataReaderByEntity and DataReader**:

  **Request**:

  ```
  {
    "workspaceId": "myWorkspace",
    "entityId": "myEntity",
    "componentName": "myComponent",
    "selectedProperties": [
      "Temperature",
      "Pressure"
    ],
    "startTime": "2022-04-07T04:04:42Z",
    "endTime": "2022-04-07T04:04:45Z",
    "maxResults": 4,
    "orderByTime": "ASCENDING",
    "properties": {
        "assetId": {
            "definition": {
                "dataType": {
                    "type": "STRING"
                },
                "isExternalId": true,
                "isFinal": true,
                "isImported": false,
                "isInherited": false,
                "isRequiredInEntity": true,
                "isStoredExternally": false,
                "isTimeSeries": false
            },
            "value": {
                "stringValue": "myAssetId"
            }
        },
        "Temperature": {
            "definition": {
                "configuration": {
                    "temperatureId": "xyz123"
                },
                "dataType": {
                    "type": "DOUBLE",
                    "unitOfMeasure": "DEGC"
                },
                "isExternalId": false,
                "isFinal": false,
                "isImported": true,
                "isInherited": false,
                "isRequiredInEntity": false,
                "isStoredExternally": false,
                "isTimeSeries": true
            }
        },
        "Pressure": {
            "definition": {
                "configuration": {
                    "pressureId": "xyz456"
                },
                "dataType": {
                    "type": "DOUBLE",
                    "unitOfMeasure": "MPA"
                },
                "isExternalId": false,
                "isFinal": false,
                "isImported": true,
                "isInherited": false,
                "isRequiredInEntity": false,
                "isStoredExternally": false,
                "isTimeSeries": true
            }
        }
    }
  }
  ```

  **Response**:

  ```
  {
    "propertyValues": [
      {
        "entityPropertyReference": {
          "entityId": "myEntity",
          "componentName": "myComponent",
          "propertyName": "Temperature"
        },
        "values": [
          {
            "time": "2022-04-07T04:04:42Z",
            "value": {
              "doubleValue": 588.168
            }
          },
          {
            "time": "2022-04-07T04:04:43Z",
            "value": {
              "doubleValue": 592.4224
            }
          }
        ]
      }
    ],
    "nextToken": "qwertyuiop"
  }
  ```
+ **AttributePropertyValueReaderByEntity**:

  **Request**:

  ```
  {
    "workspaceId": "myWorkspace",
    "entityId": "myEntity",
    "componentName": "myComponent",
    "selectedProperties": [
      "manufacturer",
    ],
    "properties": {
      "assetId": {
        "definition": {
          "dataType": {
              "type": "STRING"
          },
          "isExternalId": true,
          "isFinal": true,
          "isImported": false,
          "isInherited": false,
          "isRequiredInEntity": true,
          "isStoredExternally": false,
          "isTimeSeries": false
        },
        "value": {
            "stringValue": "myAssetId"
        }
      },
      "manufacturer": {
        "definition": {
          "dataType": {
              "type": "STRING"
          },
          "configuration": {
              "manufacturerPropId": "M001"
          },
          "isExternalId": false,
          "isFinal": false,
          "isImported": false,
          "isInherited": false,
          "isRequiredInEntity": false,
          "isStoredExternally": true,
          "isTimeSeries": false
        }
      }
    }
  }
  ```

  **Response**:

  ```
  {
    "propertyValues": {
      "manufacturer": {
        "propertyReference": {
          "propertyName": "manufacturer",
          "entityId": "myEntity",
          "componentName": "myComponent"
        },
        "propertyValue": {
          "stringValue": "Amazon"
        }
      }
    }
  }
  ```
+ **DataWriter**:

  **Request**:

  ```
  {
    "workspaceId": "myWorkspaceId",
    "properties": {
      "myEntity": {
        "Temperature": {
            "definition": {
                "configuration": {
                    "temperatureId": "xyz123"
                },
                "dataType": {
                    "type": "DOUBLE",
                    "unitOfMeasure": "DEGC"
                },
                "isExternalId": false,
                "isFinal": false,
                "isImported": true,
                "isInherited": false,
                "isRequiredInEntity": false,
                "isStoredExternally": false,
                "isTimeSeries": true
            }
        }
      }
    },
    "entries": [
      {
        "entryId": "myEntity",
        "entityPropertyReference": {
          "entityId": "myEntity",
          "componentName": "myComponent",
          "propertyName": "Temperature"
        },
        "propertyValues": [
          {
            "timestamp": 1626201120,
            "value": {
              "doubleValue": 95.6958
            }
          },
          {
            "timestamp": 1626201132,
            "value": {
              "doubleValue": 80.6959
            }
          }
        ]
      }
    ]
  }
  ```

  **Response**:

  ```
  {
      "errorEntries": [
          {
              "errors": [
                  {
                      "errorCode": "409",
                      "errorMessage": "Conflict value at same timestamp",
                      "entry": {
                          "entryId": "myEntity",
                          "entityPropertyReference": {
                              "entityId": "myEntity",
                              "componentName": "myComponent",
                              "propertyName": "Temperature"
                          },
                          "propertyValues": [
                              "time": "2022-04-07T04:04:42Z",
                              "value": {
                                  "doubleValue": 95.6958
                              }
                          ]
                      }
                  }
              ]
          }
      ]
  }
  ```