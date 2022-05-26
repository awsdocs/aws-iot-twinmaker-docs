--------

--------

# Getting started with AWS IoT TwinMaker<a name="twinmaker-gs"></a>

The topics in this section describe how to do the following\. 
+ Create and set up a new workspace\.
+ Create an entity and add a component to it\.

Prerequisites:

To create your first workspace and scene, you need the following AWS resources\.
+ An [AWS account](http://aws.amazon.com)
+ If you don't choose to let AWS IoT TwinMaker automatically create a new IAM service role, you must specify one that you have already created\. This service role must have an attached policy that grants permission for the service to read and write to an Amazon S3 bucket\. AWS IoT TwinMaker uses this role to access other services on your behalf\. You will also need to assign a trust relationship between this role and AWS IoT TwinMaker so that the service can assume the role\. If your twin interacts with other AWS services, add the necessary permissions for those services as well\.

  For instructions on creating and managing this service role, see [Create and manage a service role for AWS IoT TwinMaker](twinmaker-gs-service-role.md)\.

  For more information about IAM service roles, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.

**Note**  
This section shows you how to create a simple workspace with a single resource\. For a fully featured quick start, see the setup instructions in [AWS IoT TwinMaker samples](https://github.com/aws-samples/aws-iot-twinmaker-samples)\.

**Topics**
+ [Create and manage a service role for AWS IoT TwinMaker](twinmaker-gs-service-role.md)
+ [Create a workspace](twinmaker-gs-workspace.md)
+ [Create your first entity](twinmaker-gs-entity.md)