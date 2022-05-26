--------

--------

# Add color\-overlay augmented UI widgets to your scene<a name="scenes-editing-add-color-widget"></a>

Color overlay widgets can change the color of an object or display icons around objects, under conditions that you define\. For example, you can create a color widget that changes the color of a cookie mixer in your scene based on the mixer's temperature data\.

Use the following procedure to add color overlay widgets to a selected object\.

1. Select an object in the hierarchy that you want to add a widget to\. Press the **\+** button and then choose Color Overlay\.

1. To add a new visual rule group, in the Inspector panel for the object of the Visual Group ID, choose **ColorRule**\.

1. Select the entityID, ComponentName, or PropertyName you want to bind the color overlay to, and then choose **CreateFrameLabel**\.

## Create visual rules for your scenes<a name="scenes-editing-add-visual-rules"></a>

You can use visual rule maps to specify the data driven conditions that change the visual appearance of an augmented UI widget, such as a tag or a color overlay\. There are sample rules provided, but you can also create your own\. The following example shows visual rule\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/scene-topic-temp-sample-rule.png)

In the image **rule\_1**, it shows a rule for when the data property **temperature** is checked against a certain value\. For example if the temperature is greater than or equal to 40, the state will change the appearance of the tag to a red circle\. The **target**, when chosen in the Grafana dashboard, populates a detail panel that is configured to use the same data source\.

The following procedure shows you how to add a new visual rule group for the mesh colorization augmented UI layer\.

1. Under the rules tab in the console, enter a name such as ColorRule in the text field and choose Add **New Rule Group**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/scenes-new-vis-rule-create.png)

1. Define a new rule for your use case\. For example, you can create one based on temperature, where the temperature is less than 20\. Use the following syntax for rule expressions: Less than is **<**, greater than is **>**, less than equal is **=>**, greater than equal is **=<**, and equal is **==**\.

1. Set the target to a color\. To define a color, such as `#fcba03`, use hex values\.\. For more information about hex values, see [Hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal)\.