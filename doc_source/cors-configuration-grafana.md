--------

--------

# CORS Configuration for Grafana scene viewer<a name="cors-configuration-grafana"></a>

AWS IoT TwinMaker Grafana plugin requires a proper CORS (Cross-origin resource sharing) configuration that allows the Grafana user interface to load resources from the Amazon S3 bucket. Without the CORS configuration, you will receive an error message as "Load 3D Scene failed with Network Failure" on the Scene viewer since the Grafana domain couldn't access the resources in the Amazon S3 bucket.

For the CORS configuration of the Amazon S3 bucket, 

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the **Buckets** list, choose the name of the bucket that you use as your TwinMaker workspace's resource bucket\.

1. Choose **Permissions**\.

1. In the **Cross\-origin resource sharing \(CORS\)** section, choose **Edit**\.

1. In the **CORS configuration editor** text box, type or copy and paste the following JSON CORS configuration by replacing the Grafana workspace domain `<GRAFANA-WORKSPACE-DOMAIN>` with yours e.g. `g-1234567890.grafana-workspace.REGION.amazonaws.com`\. Please note that you keep the asterisk `*` character at the beginning.

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST",
            "DELETE",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*<GRAFANA-WORKSPACE-DOMAIN>"
        ],
        "ExposeHeaders": [
            "ETag"
        ]
    }
]
```

1. Choose **Save changes**\.

For more information about configuring CORS for Amazon S3 buckets, see [Using cross-origin resource sharing (CORS)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cors.html)\.