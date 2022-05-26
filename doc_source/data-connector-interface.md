--------

--------

# AWS IoT TwinMaker data connector interface<a name="data-connector-interface"></a>

AWS IoT TwinMaker uses a connector\-based architecture so that you can connect data from your own data store to AWS IoT TwinMaker\. This means you don't need to migrate data prior to using AWS IoT TwinMaker\. Currently, AWS IoT TwinMaker supports first\-party connectors for AWS IoT SiteWise\. If you store modeling and property data in AWS IoT SiteWise, then you don’t need to implement your own connectors\. If you store your modeling or property data in other data stores, such as Timestream, DynamoDB, or Snowflake, then you must implement Lambda connectors with the AWS IoT TwinMaker data connector interface so that AWS IoT TwinMaker can invoke your connector when necessary\. 

**Topics**
+ [AWS IoT TwinMaker data connectors](data-connector-interfaces.md)