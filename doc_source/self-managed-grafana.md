--------

--------

# Self\-managed Grafana<a name="self-managed-grafana"></a>

You can choose to host your own infrastructure to run Grafana\. For information about running Grafana locally on your machine, see [Install Grafana](https://grafana.com/docs/grafana/latest/installation/)\. The AWS IoT TwinMaker plugin is available on the public Grafana catalog\. For information about installing this plugin in your Grafana environment, see [AWS IoT TwinMaker App](https://grafana.com/grafana/plugins/grafana-iot-twinmaker-app/?tab=installation)\.

When you run Grafana locally you can't easily share dashboards or provide access to multiple users\. For a scripted quick start guide about sharing dashboards using local Grafana, see [AWS IoT TwinMaker samples repository](https://github.com/aws-samples/aws-iot-twinmaker-samples)\. This resource walks you through hosting a Grafana environment on Cloud9 and Amazon EC2 on a public endpoint\.

You must determine which authentication provider you'll use for configuring TwinMaker datasources\. You configure the credentials for the environment based on the default credentials chain \(see [Using the Default Credential Provider Chain](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html#credentials-default)\)\. The default credentials can be the permanent credentials of any IAM user or role\. For example, if you're running Grafana on Amazon EC2 , the default credentials chain has access to the [Amazon EC2 execution role](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html), which would then be your authentication provider\. The IAM Amazon Resource Name \(ARN\) of the authentication provider is required in the steps to [Creating a dashboard IAM role](dashboard-IAM-role.md)\.