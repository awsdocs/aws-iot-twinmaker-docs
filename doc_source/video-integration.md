--------

--------

# AWS IoT TwinMaker video integration<a name="video-integration"></a>

Video cameras present a good opportunity for digital twin simulation\. You can use AWS IoT TwinMaker to simulate your camera's location and physical conditions\. Create entities in AWS IoT TwinMaker for your on\-site cameras, and use video components to stream live video and metadata from your site to your AWS IoT TwinMaker scene or to a Grafana dashboard\.

AWS IoT TwinMaker can capture video from edge devices in two ways\. You can stream video from edge devices with the edge connector for Kinesis video stream, or you can save video on the edge device and initiate video uploading with MQTT messages\. Use this component to stream video data from your devices for use with AWS IoT services\. To generate the required resources and deploy the edge connector for Kinesis Video Streams, see the [Getting started with the edge connector for Kinesis video stream](https://github.com/awslabs/aws-iot-greengrass-edge-connector-for-kinesis-video-stream/tree/main/gettingstarted) on GitHub\. For more information about the AWS IoT Greengrass component, see the AWS IoT Greengrass documentation on [edge connector for Kinesis Video Streams](https://docs.aws.amazon.com/greengrass/v2/developerguide/kvs-edge-connector-component.html)\.

After you've created the required AWS IoT SiteWise models and configured the Kinesis Video Streams Greengrass component, you can stream or record video on the edge to your digital twin application in the AWS IoT TwinMaker console\. You can also view livestreams and metadata from your devices in a Grafana dashboard\. For more information about integrating Grafana and AWS IoT TwinMaker, see [AWS IoT TwinMaker Grafana dashboard integration](grafana-integration.md)\.

## Use the edge connector for Kinesis video stream to stream video in AWS IoT TwinMaker<a name="add-video-streams-to-TM"></a>

With the edge connector for Kinesis video stream, you can stream video and data to an entity in your AWS IoT TwinMaker scene\. You use a video component to do this\. To create the video component for use in your scenes, complete the following procedure\.

### Prerequisites<a name="configure-video-streams-prerequisites"></a>

Before you create the video component in your AWS IoT TwinMaker scene, make sure you've completed the following prerequisites\.
+ Created the required AWS IoT SiteWise models and assets for the edge connector for Kinesis video stream\. For more information about creating the AWS IoT SiteWise assets for the connector, see [Getting started with the edge connector for Kinesis video stream](https://github.com/awslabs/aws-iot-greengrass-edge-connector-for-kinesis-video-stream/tree/main/gettingstarted)\.
+ Deployed the Kinesis video stream edge connector on your AWS IoT Greengrass device\. For more information about deploying the Kinesis video stream edge connector component, see the deployment [README](https://github.com/awslabs/aws-iot-greengrass-edge-connector-for-kinesis-video-stream/blob/main/README.md)\.

### Create video components for AWS IoT TwinMaker scenes<a name="create-video-components"></a>

Complete the following steps to create the edge connector for the Kinesis video stream component for your scene\.

1. In the AWS IoT TwinMaker console, open the scene you want to add the video component to\.

1. After the scene is opens, choose an existing entity or create the entity you want to add the component to, and then choose **Add component**\.

1. In the **Add component** pane, enter a name for the component, and for the **Type**, choose **com\.amazon\.iotsitewise\.connector\.edgevideo**\.

1. Choose an **Asset Model** by selecting the name of the AWS IoT SiteWise camera model you created\. This name should have the following format: `EdgeConnectorForKVSCameraModel-0abc`, where the string of letters and numbers at the end matches your own asset name\.

1. For **Asset**, choose the AWS IoT SiteWise camera assets you want to stream video from\. A small window appears showing you a preview of the current video stream\.
**Note**  
To test your video streaming, choose **test**\. This test sends out an MQTT event to initiate video live streaming\. Wait for a few moments to see the video show up in the player\.

1. To add the video component to your entity, choose **Add component**\.

## Add video and metadata from Kinesis video stream to a Grafana dashboard<a name="configure-video-panel-Grafana"></a>

After you've created a video component for your entity in your AWS IoT TwinMaker scene, you can configure the video panel in Grafana to see live streams\. Make sure you have properly integrated AWS IoT TwinMaker with Grafana\. For more information, see [AWS IoT TwinMaker Grafana dashboard integration](grafana-integration.md)\.

**Important**  
To view video in your Grafana dashboard, you must make sure the Grafana datasoucres have the proper IAM permissions\. To create the required role and policy see [Creating a dashboard IAM role](dashboard-IAM-role.md)\.

Complete the following steps to see Kinesis Video Streams and metadata in your Grafana dashboard\.

1. Open the AWS IoT TwinMaker dashboard\.

1. Choose **Add pannel**, and then choose **Add an empty panel**\.

1. From the panels list, choose the **AWS IoT TwinMaker video player** panel\.

1. In the **AWS IoT TwinMaker video player** panel, enter the **stream name** of the **KinesisVideoStreamName**, with the name of the Kinesis video stream you want to stream video from\.
**Note**  
To stream metadata to the Grafana video panel, you must first have created an entity with a video streaming component\.

1. **Optional:** To stream metadata from AWS IoT SiteWise assets to the video player, for **Entity**, choose the AWS IoT TwinMaker entity that you created in your AWS IoT TwinMaker scene\. For the **Component name**, choose the video component you created for the entity in your AWS IoT TwinMaker scene\.