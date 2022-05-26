--------

--------

# Create and manage a service role for AWS IoT TwinMaker<a name="twinmaker-gs-service-role"></a>

AWS IoT TwinMaker requires that you use a service role to allow it to access resources in other services on your behalf\. This role must have a trust relationship with AWS IoT TwinMaker\. When you create a workspace, you must assign this role to the workspace\. This topic contains example policies that show you how to configure permissions for common scenarios\.

## Assign trust<a name="twinmaker-gs-service-role-trust"></a>

The following policy establishes a trust relationship between your role and AWS IoT TwinMaker\. Assign this trust relationship to the role that you use for your workspace\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "iottwinmaker.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Amazon S3 permissions<a name="twinmaker-gs-service-role-s3"></a>

The following policy allows your role to read and delete from and write to an Amazon S3 bucket\. Workspaces store resources in Amazon S3, so the Amazon S3 permissions are required for all workspaces\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucket*",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::*/DO_NOT_DELETE_WORKSPACE_*"
      ]
    }
  ]
}
```

**Note**  
When you create a workspace, AWS IoT TwinMaker creates a file in your Amazon S3 bucket that indicates it's being used by a workspace\. This policy gives AWS IoT TwinMaker permission to delete that file when you delete the workspace\.  
AWS IoT TwinMaker places other objects related to your workspace\. It's your responsibility to delete these objects when you delete a workspace\.

## Assign permissions to a specific Amazon S3 bucket<a name="twinmaker-gs-service-role-bucket"></a>

When you create a workspace in the AWS IoT TwinMaker console, you can choose to have AWS IoT TwinMaker create an Amazon S3 bucket for you\. You can find information about this bucket by using the following AWS CLI command\.

```
  aws iottwinmaker get-workspace --workspace-id workspace name              
```

The following example shows the format of the output of this command\.

```
{
    "arn": "arn:aws:iottwinmaker:region:account Id:workspace/workspace name",
    "creationDateTime": "2021-11-30T11:30:00.000000-08:00",
    "description": "",
    "role": "arn:aws:iam::account Id:role/service role name",
    "s3Location": "arn:aws:s3:::bucket name",
    "updateDateTime": "2021-11-30T11:30:00.000000-08:00",
    "workspaceId": "workspace name"
}
```

To update your policy so that it assigns permissions for a specific Amazon S3 bucket, use the value of *bucket name*\.

The following policy allows your role to read and delete from and write to a specific Amazon S3 bucket\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucket*",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::bucket name",
        "arn:aws:s3:::bucket name/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::iottwinmakerbucket/DO_NOT_DELETE_WORKSPACE_*"
      ]
    }
  ]
}
```

## Permissions for built\-in connectors<a name="twinmaker-gs-service-role-sitewise"></a>

If your workspace interacts with other AWS services by using built\-in connectors, you must include permissions for those services in this policy\. If you use the **com\.amazon\.iotsitewise\.connector** component type, you must include permissions for AWS IoT SiteWise\. For more information about component types, see [Using and creating component types](twinmaker-component-types.md)\. 

**Note**  
If you interact with other AWS services by using a custom component type, you must grant the role permission to run the Lambda function that implements the function in your component type\. For more information, see [Permissions for a connector to an external data source](#twinmaker-gs-service-role-external)\.

The following example shows how to include AWS IoT SiteWise in your policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucket*",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::bucket name",
        "arn:aws:s3:::bucket name/*"
      ]
    },
    {
        "Effect": "Allow",
        "Action": [
            "iotsitewise:DescribeAsset"
        ],
        "Resource": "asset ARN"
        },
    {
        "Effect": "Allow",
        "Action": [
            "iotsitewise:DescribeAssetModel"
        ],
        "Resource": "asset model ARN"
        },
    {
      "Effect": "Allow",
      "Action": [
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::*/DO_NOT_DELETE_WORKSPACE_*"
      ]
    }
  ]
}
```

If you use the **com\.amazon\.iotsitewise\.connector** component type and need to read property data from AWS IoT SiteWise, you must include the following permission in your policy\.

```
...
{
    "Action": [
        "iotsitewise:GetPropertyValueHistory",
    ],
    "Resource": [
        "AWS IoT SiteWise asset resource ARN"
    ],
    "Effect": "Allow"
},
...
```

If you use the **com\.amazon\.iotsitewise\.connector** component type and need to write property data to AWS IoT SiteWise, you must include the following permission in your policy\.

```
...
{
    "Action": [
        "iotsitewise:BatchPutPropertyValues",
    ],
    "Resource": [
        "AWS IoT SiteWise asset resource ARN"
    ],
    "Effect": "Allow"
},
...
```

If you use the **com\.amazon\.iotsitewise\.connector\.edgevideo** component type, you must include permissions for AWS IoT SiteWise and Kinesis Video Streams\. The following example policy shows how to include AWS IoT SiteWise and Kinesis Video Streams permissions in your policy\. 

```
...
{
    "Action": [
        "iotsitewise:DescribeAsset",
        "iotsitewise:GetAssetPropertyValue"
    ],
    "Resource": [
        "AWS IoT SiteWise asset resource ARN for the Edge Connnector for Kinesis Video Streams"
    ],
    "Effect": "Allow"
},
{
    "Action": [
        "iotsitewise:DescribeAssetModel"
    ],
    "Resource": [
        "AWS IoT SiteWise model resource ARN for the Edge Connnector for Kinesis Video Streams"
    ],
    "Effect": "Allow"
},
{
    "Action": [
        "kinesisvideo:DescribeStream"
    ],
    "Resource": [
        "Kinesis Video Streams stream ARN"
    ],
    "Effect": "Allow"
},
...
```

## Permissions for a connector to an external data source<a name="twinmaker-gs-service-role-external"></a>

If you create a component type that uses a function that connects to an external data source, you must give your service role permission to use the Lambda function that implements the function\. For more information about creating component types and functions, see [Using and creating component types](twinmaker-component-types.md)\.

The following example gives permission to your service role to use a Lambda function\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucket*",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::bucket name",
        "arn:aws:s3:::bucket name/*"
      ]
    },
    {
        "Action": [
            "lambda:invokeFunction"
        ],
        "Resource": [
            "Lambda function ARN"
        ],
        "Effect": "Allow"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::*/DO_NOT_DELETE_WORKSPACE_*"
      ]
    }
  ]
}
```

For more information about creating roles and assigning policies and trust relationships to them by using the IAM console, the AWS CLI, and the IAM API, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.