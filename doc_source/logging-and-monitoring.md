--------

--------

# Logging and monitoring in AWS IoT TwinMaker<a name="logging-and-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS IoT TwinMaker and your other AWS solutions\. AWS IoT TwinMaker supports the following monitoring tools to watch the service, report when something is wrong, and take automatic actions when appropriate:
+ *Amazon CloudWatch* monitors in real time your AWS resources and the applications that you run on AWS\. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. For example, you can have CloudWatch track CPU usage or other metrics for your Amazon EC2 instances and automatically launch new instances when needed\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.
+ *Amazon CloudWatch Logs* monitors, stores, and provides access to your log files from AWS IoT TwinMaker gateways, CloudTrail, and other sources\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.
+ *AWS CloudTrail* captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

**Topics**
+ [Monitoring AWS IoT TwinMaker with Amazon CloudWatch metrics](monitor-cloudwatch-metrics.md)
+ [Logging AWS IoT TwinMaker API calls with AWS CloudTrail](logging-using-cloudtrail.md)