--------

--------

# Creating an AWS IoT TwinMaker video player policy<a name="tm-video-policy"></a>

The following is a policy template with all of the video permissions you need for the AWS IoT TwinMaker plugin in Grafana:

```
{
  "Version": "2012-10-17",
  "Statement": [
    { 
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::bucketName/*",
        "arn:aws:s3:::bucketName"
        ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iottwinmaker:Get*",
        "iottwinmaker:List*"
      ],
      "Resource": [
        "arn:aws:iottwinmaker:region:accountId:workspace/workspaceId",
        "arn:aws:iottwinmaker:region:accountId:workspace/workspaceId/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iottwinmaker:ListWorkspaces",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "kinesisvideo:GetDataEndpoint",
        "kinesisvideo:GetHLSStreamingSessionURL"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iotsitewise:GetAssetPropertyValue",
        "iotsitewise:GetInterpolatedAssetPropertyValues"
      ],
      "Resource": "*"
    },
    {
       "Effect": "Allow",
       "Action": [
        "iotsitewise:BatchPutAssetPropertyValue"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "aws:ResourceTag/EdgeConnectorForKVS": "*workspaceId*"
        } 
      }
    }
  ]
}
```

For more information about the full policy, see the **All video permissions** policy template in the [Create an IAM policy](dashboard-IAM-role.md#IAM-policy) topic\.

## Scope down access to your resources<a name="scope-down-access"></a>

The Video Player panel in Grafana directly calls Kinesis Video Streams and IoT SiteWise to provide a complete video playback experience\. To avoid unauthorized access to resources that aren't associated with your AWS IoT TwinMaker workspace, add conditions to the IAM policy for your workspace dashboard role\.

## Scope down GET permissions<a name="scope-down-GET-permissions"></a>

You can scope down the access of your Amazon Kinesis Video Streams and AWS IoT SiteWise assets by tagging resources\. You might have already tagged your AWS IoT SiteWise camera asset based on the AWS IoT TwinMaker workspaceId to enable the video upload request feature, see [](dashboard-IAM-role.md#tagging-camera-assets)\. You can use the same tag key\-value pair to limit GET access to AWS IoT SiteWise assets, and also to tag your Kinesis Video Streams the same way\.

You can then add this condition to the kinesisvideo and iotsitewise statements in the `YourWorkspaceIdDashboardPolicy`:

```
"Condition": {
  "StringLike": {
    "aws:ResourceTag/EdgeConnectorForKVS": "*workspaceId*"
  } 
}
```

### Real\-life use case: Grouping cameras<a name="use-cases"></a>

In this scenario, you have a large array of cameras monitoring the process of baking cookies in a factory\. Batches of cookie batter are made in the Batter Room, batter is frozen in the Freezer Room, and cookies are baked in the Baking Room\. There are cameras in each of these rooms with different teams of operators separately monitoring each process\. You want each group of operators to be authorized for their respective room\. When building a digital twin for the cookie factory, a single workspace is used, but the camera permissions need to be scoped by room\.

You can achieve this permission separation by tagging groups of cameras based on their groupingId\. In this scenario, the groupingIds are BatterRoom, FreezerRoom, and BakingRoom\. The camera in each room is connected to Kinesis Video Streams and should have a tag with: Key = `EdgeConnectorForKVS`, Value = `BatterRoom`\. The value can be a list of groupings delimited by any of the following characters:`. : + = @ _ / -`

To amend the `YourWorkspaceIdDashboardPolicy`, use the following policy statements:

```
...,
{
  "Effect": "Allow",
  "Action": [
    "kinesisvideo:GetDataEndpoint",
    "kinesisvideo:GetHLSStreamingSessionURL"
  ],
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "aws:ResourceTag/EdgeConnectorForKVS": "*groupingId*"
    } 
  }
},
{
  "Effect": "Allow",
  "Action": [
    "iotsitewise:GetAssetPropertyValue",
    "iotsitewise:GetInterpolatedAssetPropertyValues"
  ],
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "aws:ResourceTag/EdgeConnectorForKVS": "*groupingId*"
    } 
  }
},
...
```

These statements restrict streaming video playback and AWS IoT SiteWise property history access to specific resources in a grouping\. The `groupingId` is defined by your use case\. In the our scenario, it would be the roomId\.

## Scope down AWS IoT SiteWise BatchPutAssetPropertyValue permission<a name="scope-down-BatchPutAssetPropertyValue"></a>

Providing this permission turns on the Video Player\. When you upload video, you can specify a time range and submit the request from by choosing **Submit** on the panel on the Grafana dashboard\. 

To give iotsitewise:BatchPutAssetPropertyValue permissions, use the default policy: 

```
...,
{
  "Effect": "Allow",
  "Action": [
    "iotsitewise:BatchPutAssetPropertyValue"
  ],
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "aws:ResourceTag/EdgeConnectorForKVS": "*workspaceId*"
    } 
  }
},
...
```

By using this policy, users can call BatchPutAssetPropertyValue for any property on the AWS IoT SiteWise camera asset\. You can restrict authorization for a specific propertyId by specifying it in the statement’s condition\.

```
{
  ...
  "Condition": {
    "StringEquals": {
      "iotsitewise:propertyId": "propertyId"
    } 
  }
  ...
}
```

The Video Player panel in Grafana ingests data into the measurement property, named VideoUploadRequest, to initiate the uploading of video from the edge cache to Kinesis Video Streams\. Find the propertyId of this property in the AWS IoT SiteWise Console\. To amend the `YourWorkspaceIdDashboardPolicy`, use the following policy statement: 

```
...,
{
  "Effect": "Allow",
  "Action": [
    "iotsitewise:BatchPutAssetPropertyValue"
  ],
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "aws:ResourceTag/EdgeConnectorForKVS": "*workspaceId*"
    },
    "StringEquals": {
      "iotsitewise:propertyId": "VideoUploadRequestPropertyId"
    }
  }
},
...
```

This statement restricts ingesting data to a specific property of your tagged AWS IoT SiteWise camera asset\. For more information, see [How AWS IoT SiteWise works with IAM](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/security_iam_service-with-iam.html)\. 