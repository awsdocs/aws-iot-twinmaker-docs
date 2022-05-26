--------

--------

# Create your first entity<a name="twinmaker-gs-entity"></a>

To create your first entity, use the following steps\.

1. On the **Workspaces** page, choose your workspace, and then in the left pane choose **Entities**\.

1. On the **Entities** page, choose **Create**, and then choose **Create entity**\.  
![\[When you choose Create, the Create entity option appears.\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/CreateEntity.png)

1. In the **Create an entity** window, enter a name for your entity\. This example uses a **CookieMixer** entity\.

1. \(Optional\) Enter a description for your entity\.

1. Choose **Create entity**,

Entities contain data about each item in your workspace\. You put data into entities by adding components\. AWS IoT TwinMaker provides the following built\-in component types\.
+ **Parameters**: Adds a set of key\-value properties\.
+ **Document**: Adds a name and a URL for a document that contains information about the entity\.
+ **Alarms**: Connects to an alarm time\-series data source\.
+ **SiteWise connector**: Pulls time\-series properties that are defined in an AWS IoT SiteWise asset\.
+ **Edge Connector for Kinesis Video Streams AWS IoT Greengrass**: Pulls video data from the Edge Connector for KVS AWS IoT Greengrass\. For more information, see [AWS IoT TwinMaker video integration](video-integration.md)\.

You can see these component types and their definitions by choosing **Component types** in the left pane\. You can also create a new component type on the **Component types** page\. For more information about creating component types, see [Using and creating component types](twinmaker-component-types.md)\.

In this example, we create a simple document component that adds descriptive information about your entity\.

1. On the **Entities** page, chooose the entity, and then choose add component\.  
![\[When you choose your entity, the Add component button appears.\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/AddComponent.png)

1. In the **Add component** window, enter a name for your component\. Since this example uses a cookie mixer entity, we enter **MixerDescription** in the **Name** field\.

1. Choose **Add**\. The following window appears\.  
![\[When you choose Add, the Add a doc button appears.\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/DocumentComponent.png)

1. Choose **Add a doc**\. The following fields appear in the window\.  
![\[When you choose Add a doc, the Name and External Urlfields appear.\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/DocumentComponent.png)

   Enter values for **Name** and **External Url**\. With the documents component, you can store a list of external Urls that contain important information about the entity\.

1. Choose **Add component**\.

You're now ready to create your first scene\. For instructions on how to do this, see [Creating and editing AWS IoT TwinMaker scenes](scenes.md)\.