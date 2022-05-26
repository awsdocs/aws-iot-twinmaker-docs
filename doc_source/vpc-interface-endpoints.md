--------

--------

# AWS IoT TwinMaker and interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-interface-endpoints"></a>

You can establish a private connection between your virtual private cloud \(VPC\) and AWS IoT TwinMaker by creating an *interface VPC endpoint*\. Interface endpoints are powered by [AWS PrivateLink](http://aws.amazon.com/privatelink), which you can use to privately access AWS IoT TwinMaker APIs without an internet gateway, network address translation \(NAT\) device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with AWS IoT TwinMaker APIs\. Traffic between your VPC and AWS IoT TwinMaker doesn't leave the Amazon network\. 

Each interface endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in your subnets\. 

For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

## Considerations for AWS IoT TwinMaker VPC endpoints<a name="vpc-endpoint-considerations"></a>

Before you set up an interface VPC endpoint for AWS IoT TwinMaker, review [Interface endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-limitations) in the *Amazon VPC User Guide*\.

AWS IoT TwinMaker supports making calls to all of its API actions from your VPC\.
+ For data plane API operations, use the following endpoint:

  ```
  data.iottwinmaker.region.amazonaws.com
  ```

  The data plane API operations include the following:
  + [GetPropertyValue](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValue.html)
  + [GetPropertyValueHistory](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetPropertyValueHistory.html)
  + [BatchPutPropertyValues](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_BatchPutPropertyValues.html)
+ For the control plane API operations, use the following endpoint:

  ```
  api.iottwinmaker.region.amazonaws.com
  ```

  The supported control plane API operations include the following:
  + [CreateComponentType](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_CreateComponentType.html)
  + [CreateEntity](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_CreateEntity.html)
  + [CreateScene](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_CreateScene.html)
  + [CreateWorkspace](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_CreateWorkspace.html)
  + [DeleteComponentType](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DeleteComponentType.html)
  + [DeleteEntity](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DeleteEntity.html)
  + [DeleteScene](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DeleteScene.html)
  + [DeleteWorkspace](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_DeleteWorkspace.html)
  + [GetComponentType](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetComponentType.html)
  + [GetEntity](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetEntity.html)
  + [GetScene](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetScene.html)
  + [GetWorkspace](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_GetWorkspace.html)
  + [ListComponentTypes](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_ListComponentTypes.html)
  + [ListComponentTypes](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_ListComponentTypes.html)
  + [ListEntities](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_ListEntities.html)
  + [ListScenes](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_ListScenes.html)
  + [ListTagsForResource](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_ListTagsForResource.html)
  + [ListWorkspaces](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_ListWorkspaces.html)
  + [TagResource](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_TagResource.html)
  + [UntagResource](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_UntagResource.html)
  + [UpdateComponentType](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_UpdateComponentType.html)
  + [UpdateEntity](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_UpdateEntity.html)
  + [UpdateScene](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_UpdateScene.html)
  + [UpdateWorkspace](https://docs.aws.amazon.com/iot-twinmaker/latest/apireference/API_UpdateWorkspace.html)

## Creating an interface VPC endpoint for AWS IoT TwinMaker<a name="vpc-endpoint-create"></a>

You can create a VPC endpoint for the AWS IoT TwinMaker service by using either the Amazon VPC console or the AWS Command Line Interface \(AWS CLI\)\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

Create a VPC endpoint for AWS IoT TwinMaker that uses the following service name\. 
+ For data plane API operations, use the following service name: 

  ```
  com.amazonaws.region.iottwinmaker.data
  ```
+ For control plane API operations, use the following service name: 

  ```
  com.amazonaws.region.iottwinmaker.api
  ```

If you enable private DNS for the endpoint, you can make API requests to AWS IoT TwinMaker by using its default DNS name for the Region, for example, `iottwinmaker.us-east-1.amazonaws.com`\. 

For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

AWS IoT TwinMaker PrivateLink is supported in the following regions:
+ **us\-east\-1**

  The ControlPlane service is supported in the following availability zones: `use1-az1`, `use1-az2`, and `use1-az6`\.

  The DataPlane service is supported in the following availability zones: `use1-az1`, `use1-az2`, and `use1-az4`\.
+ **us\-west\-2**

  The ControlPlane and DataPlane services are supported in the following availability zones: `usw2-az1`, `usw2-az2`, and `usw2-az3`\. 
+ **eu\-west\-1**
+ **eu\-central\-1**
+ **ap\-southeast\-1**
+ **ap\-southeast\-2**

For more information on availability zones, see [Availability Zone IDs for your AWS resources \- AWS Resource Access Manager](https://docs.aws.amazon.com/ram/latest/userguide/working-with-az-ids.html)\.

## Accessing AWS IoT TwinMaker through an interface VPC endpoint<a name="vpc-endpoint-access"></a>

When you create an interface endpoint, AWS IoT TwinMaker generates endpoint\-specific DNS hostnames that you can use to communicate with AWS IoT TwinMaker\. The private DNS option is enabled by default\. For more information, see [Using private hosted zones](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-private-hosted-zones) in the *Amazon VPC User Guide*\.

If you enable private DNS for the endpoint, you can make API requests to AWS IoT TwinMaker through one of the following VPC endpoints\.
+ For the data plane API operations, use the following endpoint\. Replace *region* with your AWS Region\.

  ```
  data.iottwinmaker.region.amazonaws.com
  ```
+ For the control plane API operations, use the following endpoint\. Replace *region* with your AWS Region\.

  ```
  api.iottwinmaker.region.amazonaws.com
  ```

If you disable private DNS for the endpoint, you must do the following to access AWS IoT TwinMaker through the endpoint:
+ Specify the VPC endpoint URL in API requests\.
  + For the data plane API operations, use the following endpoint URL\. Replace *vpc\-endpoint\-id* and *region* with your VPC endpoint ID and Region\.

    ```
    vpc-endpoint-id.data.iottwinmaker.region.vpce.amazonaws.com
    ```
  + For the control plane API operations, use the following endpoint URL\. Replace *vpc\-endpoint\-id* and *region* with your VPC endpoint ID and Region\.

    ```
    vpc-endpoint-id.api.iottwinmaker.region.vpce.amazonaws.com
    ```
+ Disable host prefix injection\. The AWS CLI and AWS SDKs prepend the service endpoint with various host prefixes when you call each API operation\. This causes the AWS CLI and AWS SDKs to produce invalid URLs for AWS IoT TwinMaker when you specify a VPC endpoint\.
**Important**  
You can't disable host prefix injection in AWS CLI or AWS Tools for PowerShell\. This means that if you've disabled private DNS, you won't be able to use AWS CLI or AWS Tools for PowerShell to access AWS IoT TwinMaker through the VPC endpoint\. If you want to use these tools to access AWS IoT TwinMaker through the endpoint, enable private DNS\.

  For more information about how to disable host prefix injection in the AWS SDKs, see the following documentation sections for each SDK:
  + [AWS SDK for C\+\+](https://sdk.amazonaws.com/cpp/api/LATEST/struct_aws_1_1_client_1_1_client_configuration.html#a3579c1a2f2e1c9d54e99c59d27643499)
  + [AWS SDK for Go](https://docs.aws.amazon.com/sdk-for-go/api/aws/#Config.WithDisableEndpointHostPrefix)
  + [AWS SDK for Go v2](https://docs.aws.amazon.com/sdk-for-go/v2/api/aws/#Config)
  + [AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/ClientConfiguration.html#setDisableHostPrefixInjection-boolean-)
  + [AWS SDK for Java 2\.x](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/core/client/config/SdkAdvancedClientOption.html)
  + [AWS SDK for JavaScript](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Config.html#hostPrefixEnabled-property)
  + [AWS SDK for \.NET](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/Runtime/TClientConfig.html)
  + [AWS SDK for PHP](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.AwsClient.html#___construct)
  + [AWS SDK for Python \(Boto3\)](https://botocore.amazonaws.com/v1/documentation/api/latest/reference/config.html)
  + [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/IoTSiteWise/Client.html#initialize-instance_method)

For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

## Creating a VPC endpoint policy for AWS IoT TwinMaker<a name="vpc-endpoint-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to AWS IoT TwinMaker\. The policy specifies the following information:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

**Example: VPC endpoint policy for AWS IoT TwinMaker actions**  
The following is an example of an endpoint policy for AWS IoT TwinMaker\. When attached to an endpoint, this policy grants access to the listed AWS IoT TwinMaker actions for the IAM user `iottwinmakeradmin` in the AWS account `123456789012` on all resources\.

```
{
   "Statement":[ 
      {
        "Principal": {
            "AWS": "arn:aws:iam::123456789012:user/role"
                },
         "Resource": "*",
         "Effect":"Allow",
         "Action":[
            "iottwinmaker:CreateEntity",
            "iottwinmaker:GetScene",
            "iottwinmaker:ListEntities"
         ]
        }
    ]
}
```