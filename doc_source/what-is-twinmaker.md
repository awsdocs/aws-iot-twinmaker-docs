--------

--------

# What is AWS IoT TwinMaker?<a name="what-is-twinmaker"></a>

AWS IoT TwinMaker is an AWS IoT service that you can use to build operational digital twins of physical and digital systems\. AWS IoT TwinMaker creates digital visualizations using measurements and analysis from a variety of real\-world sensors, cameras, and enterprise applications to help you keep track of your physical factory, building, or industrial plant\. You can use this real\-world data to monitor operations, diagnose and correct errors, and optimize operations\.

 A digital twin is a live digital representation of a system and all of its physical and digital components\. It is dynamically updated with data to mimic the true structure, state, and behavior of the system\. You can use it to drive business outcomes\. 

End users interact with data from your digital twin by using a user interface application\.

## How it works<a name="roci-how-it-works"></a>

To fulfill the minimum requirements for creating a digital twin, you must do the following\.
+ Model devices, equipment, spaces, and processes in a physical location\.
+ Connect these models to data sources that store important contextual information, such as sensor data camera feeds\.
+ Create visualizations that help users understand the data and insights in order to make business decisions more efficiently\.
+ Make digital twins available to end users to drive business outcomes\.

AWS IoT TwinMaker addresses these challenges by providing the following capabilities\.
+ Entity component system knowledge graph: AWS IoT TwinMaker provides tools for modeling devices, equipment, spaces, and processes in a knowledge graph\.

  This knowledge graph contains metadata about the system and can connect to data in different locations\. AWS IoT TwinMaker provides built\-in connectors for data stored in AWS IoT SiteWise and Kinesis Video Streams\. You can also create custom connectors to data stored in other locations\.

  The knowledge graph and connectors together provide a single interface for querying data in disparate locations\.
+ Scene composer: The AWS IoT TwinMaker console provides a scene composition tool for creating scenes in 3D\. You upload your previously built 3D/CAD models, optimized for web display and converted to \.gltf or \.glb format\. You then use the scene composer to place multiple models in a single scene, creating visual representations of their operations\.

  You can also overlay data in the scene\. For example, you can create a tag in a scene location that connects to temperature data from a sensor\. This associates the data with the location\.
+ Applications: AWS IoT TwinMaker provides a plug\-in for Grafana and Amazon Managed Grafana that you can use to build dashboard applications for end users\. 

## Key concepts and components<a name="roci-glossary"></a>

The following diagram illustrates how the key concepts of AWS IoT TwinMaker fit together\.

![\[AWS IoT TwinMaker workspaces consist of components, entities, scenes, and resources. Scenes consist of nodes. Nodes can be empty or contain a tag, light, or shader.\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/what-is-twinmaker.png)

**Note**  
Asterisks \(\*\) in the diagram indicate one\-to\-many relationships\. For the quotas for each of these relationships, see [AWS IoT TwinMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-twinmaker.html)\.

The following sections describe the concepts illustrated in the diagram\.

### Workspace<a name="roci-gloss-workspace"></a>

A workspace is a top\-level container for your digital twin application\. You create a logical set of entities, components, scene assets, and other resources for your digital twin inside this workspace\. It also serves as a security boundary to manage access to the digital twin application and the resources it contains\. Each workspace is linked to the Amazon S3 bucket where your workspace data is stored\. You use IAM roles to restrict access to your workspace\. 

A workspace can contain multiple components, entities, scenes and resources\. A component type, entity, scene or resource exists only within one workspace\. 

### Entity\-component model<a name="roci-gloss-ecm"></a>

 AWS IoT TwinMaker provides tools that you use to model your system by using an entity\-component\-based knowledge graph\. You can use the entity\-component architecture to create a representation of your physical system\. This entity component model consists of **entities**, **components**, and *relationships*\. For more information about entity\-component systems, see [Entity component system](https://en.wikipedia.org/wiki/Entity_component_system)\.

#### Entity<a name="roci-gloss-entity"></a>

 Entities are digital representations of the elements in a digital twin that capture the capabilities of that element\. This element can be a piece of physical equipment, a concept, or a process\. Entities have **components** associated with them\. These **components** provide data and context for the associated entity\. 

 With AWS IoT TwinMaker, you can organize entities into custom hierarchies for more efficient management\. The default view of the entity and component system is hierarchical\. 

#### Component<a name="roci-gloss-component"></a>

 Components provide context and data for entities in a scene\. You add components to entities\. The lifetime of a component is tied to the lifetime of an entity\.

Components can add static data, such as a list of documents or the coordinates of a geographic location\. They can also have functions that connect to other systems, including systems that contain time series data such as AWS IoT SiteWise and other time\-series cloud historians\.

Components are defined by JSON documents that describe the connection between a data source and AWS IoT TwinMaker\. Components can describe external data sources or data sources that are built in to AWS IoT TwinMaker\. A component accesses an external datasource by using a Lambda function that is specified in the JSON document\. A workspace can contain many components\. Components provide data to tags through associated entities\.

AWS IoT TwinMaker provides several built\-in components that you can add from the console\. You can also create your own custom components to connect to sources of data such as timestream telemetry and geospatial coordinates\. Examples of these include TimeStream Telemetry, Geospatial components, and connectors to third party data sources such as Snowflake\.

AWS IoT TwinMaker provides the following types of built\-in components for common use cases:
+ **Document**, such as user manuals or images located at specified URLs\.
+ **Time series**, such as sensor data from AWS IoT SiteWise\.
+ **Alarms**, such as time\-series alarms from external data sources\.
+ **Video**, from IP cameras connected to Kinesis Video Streams\.
+ **Custom components** to connect to additional data sources\. For example, you can create a custom connector to connect your AWS IoT TwinMaker entities to time\-series data stored externally\.

##### Data sources<a name="roci-gloss-component"></a>

A data source is the location of your digital twin’s source data\. AWS IoT TwinMaker supports two types of data sources:
+ **Hierarchy connectors**, which allow you to continually sync an external model to AWS IoT TwinMaker\.
+ **Time\-series connectors**, which allow you to connect to time\-series databases such as AWS IoT SiteWise\.

##### Property<a name="roci-gloss-property"></a>

Properties are the values, both static and time\-series backed, contained in components\. When you add components to entities, the properties in the component describe details about the current state of the entity\.

AWS IoT TwinMaker supports three kinds of properties:
+ **Single value, non\-time\-series properties**— These properties are typically static key\-value pairs and are directly stored in AWS IoT TwinMaker with the metadata of the associated entity\.
+ **Time\-series properties**— AWS IoT TwinMaker stores a reference to the time\-series store for these properties\. This defaults to the latest value\.
+ **Relationship properties**— These properties store a reference to another entity or component\. For example, `seen_by` is a relationship component that might relate a camera entity to another entity that is directly visualized by that camera\. 

You can query property values across heterogeneous data sources by using the unified data query interface\.

### Visualization<a name="roci-gloss-visualization"></a>

You use AWS IoT TwinMaker to augment a three\-dimensional representation of your digital twin, and then view it in Grafana\. To create scenes, use existing CAD or other 3D file types\. You then use data overlays to add relevant data for your digital twin\.

#### Scenes<a name="roci-gloss-scenes"></a>

Scenes are three\-dimensional representations that provide visual context for the data connected to AWS IoT TwinMaker\. Scenes can be created by using a single gltf \(GL Transmission Format\) or glb 3D model for the entire environment, or by using a composition of multiple models\. Scenes also include **tags** to denote points of interest in the scene\. 

Scenes are the top level containers for visualisations\. A scene consists of one or more nodes\.

A workspace can contain multiple scenes\. For example, a workspace can contain one scene for each floor of a facility\. 

#### Resources<a name="roci-gloss-resources"></a>

Scenes display resources, which are displayed as nodes in the AWS IoT TwinMaker console\. A scene can contain many resources\.

Resources are images and `glTF`\-based, three\-dimensional models used to create a scene\. A resource can represent a single piece of equipment, or a complete site\.

You place resources into a scene by uploading a \.gltf or \.glb file to your workspace resource library and then adding them to your scene\.

#### Augmented user interface<a name="roci-gloss-tag"></a>

With AWS IoT TwinMaker you can augment your scenes with data overlays that add important context and information, such as sensor data, to locations in the scene\.

**Nodes**: Nodes are instances of tags, lights, and three\-dimensional models\. They can also be empty to add structure to your scene hierarchy\. For example, you can group multiple nodes together under a single empty node\.

**Tags**: A tag is a type of node that represents data from a component \(through an entity\)\. A tag can be associated with only one component\. A tag is an annotation added to a speciﬁc `x,y,z` coordinate position of a scene\. The tag connects this scene part to the knowledge graph by using an entity property\. You can use a tag to conﬁgure the behavior or visual appearance of an item in the scene, such as an alarm\. 

**Lights**: You can add lights to a scene to bring certain objects into focus, or cast shadows on objects to indicate their physical location\.

**Three\-dimensional models**: A three\-dimensional nodel is a visual representation of a \.gltf or \.glb file imported as a resource\.

**Note**  
AWS IoT TwinMaker is not intended for use in, or in association with, the operation of any hazardous environments or critical systems that may lead to serious bodily injury or death or cause environmental or property damage\.  
Data collected through your use of AWS IoT TwinMaker should be evaluated for accuracy as appropriate for your use case\. AWS IoT TwinMaker should not be used as a substitute for human monitoring of physical systems for purposes of assessing whether such systems are operating safely\.