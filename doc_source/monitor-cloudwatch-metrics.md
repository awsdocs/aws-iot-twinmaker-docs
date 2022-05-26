--------

--------

# Monitoring AWS IoT TwinMaker with Amazon CloudWatch metrics<a name="monitor-cloudwatch-metrics"></a>

You can monitor AWS IoT TwinMaker by using CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. These statistics are kept for 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. You can also set alarms that watch for certain thresholds, and send notifications or take actions when those thresholds are met\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

AWS IoT TwinMaker publishes the metrics and dimensions listed in the following sections to the `AWS/IoTTwinMaker` namespace\.

**Tip**  
AWS IoT TwinMaker publishes metrics on a one minute interval\. When you view these metrics in graphs in the CloudWatch console, we recommend that you choose a **Period** of **1 minute** to see the highest available resolution of your metric data\.

**Contents**
+ [Metrics](#gateway-metrics)

## Metrics<a name="gateway-metrics"></a>

AWS IoT TwinMaker publishes the following metrics\.


**Metrics**  

| Metric | Description | 
| --- | --- | 
| ComponentTypeCreationFailure |  This metric reports whether the component type creation is successful\. The metric is published when a component type is in `CREATING` state\. This happens when a component type is created with the required properties in the schema initializer and these properties are instantiated with default values\.  The metric value can be either `0` for success or `1` for failure\. **Dimensions**: ComponentTypeId, WorkspaceId\.  **Units**: Count  | 
| ComponentTypeUpdateFailure |  This metric reports whether the component type update is successful\. The metric is published when a component type is in `UPDATING` state\. This happens when a component type is updated with the required properties in the schema initializer and these properties are instantiated with default values\.  The metric value can be either `0` for success or `1` for failure\. **Dimensions**: ComponentTypeId, WorkspaceId\.  **Units**: Count  | 
| EntityCreationFailure |  This metric reports whether the entity creation is successful\. The metric is published when an entity is in `CREATING` state\. This happens when an entity is created with a component\. The metric value can be either `0` for success or `1` for failure\.  **Dimensions**: EntityName, EntityId, WorkspaceId\. **Units**: Count  | 
| EntityUpdateFailure |  This metric reports whether the entity update is successful\. The metric is published when an entity is in `UPDATING` state\. This happens when an entity is updated\. The metric value can be either `0` for success or `1` for failure\. **Dimensions**: EntityName, EntityId, WorkspaceId\. **Units**: Count  | 
| EntityDeletionFailure |  This metric reports whether the entity deletion is successful\. The metric is published when an entity is in `DELETING` state\. This happens when an entity is deleted\. The metric value can be either `0` for success or `1` for failure\. **Dimensions**: EntityName, EntityId, WorkspaceId\. **Units**: Count  | 

**Tip**  
All metrics are published to the `AWS/IoTTwinMaker` namespace\.