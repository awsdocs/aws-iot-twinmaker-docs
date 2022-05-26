--------

--------

# Create a workspace<a name="twinmaker-gs-workspace"></a>

To create and configure your first workspace, use the following steps\.

1. On the [AWS IoT TwinMaker console](https://console.aws.amazon.com/iottwinmaker/home) home page, choose **Workspaces** in the left navigation pane\.

1. On the **Workspaces** page, choose **Create workspace**\.

1. On the **Create a Workspace** page, enter a name for your workspace\.

1. \(Optional\) Add a description for your workspace\.

1. Under **S3 resource**, choose **Create an S3 resource**\. This option creates an Amazon S3 bucket where AWS IoT TwinMaker stores information and resources related to the workspace\. Each workspace requires its own bucket\.

1. Under **Execution role**, choose either **Auto\-generate a new role** or the IAM role that you created as a prerequisite for this section\.

   If you choose **Auto\-generate a new role**, AWS IoT TwinMaker attaches a policy to the role that grants permission to the new service role to access other AWS services, including permission to read and write to the Amazon S3 bucket that you specify in the previous step\. For information about assigning permissions to this role, see [Create and manage a service role for AWS IoT TwinMaker](twinmaker-gs-service-role.md)\.

1. Choose **Create Workspace**\. The following banner appears at the top of the **Workspaces** page\.  
![\[After you create a workspace, the Workspaces page displays a banner. The banner contains a button that displays the IAM policy that you should add to the role that IAM users and accounts use when viewing the AWS IoT TwinMaker dashboard.\]](http://docs.aws.amazon.com/iot-twinmaker/latest/guide/images/DashboardPolicy.png)

1. Choose **Get json**\. We recommend you add the IAM policy you see to the IAM role that AWS IoT TwinMaker created for users and accounts that view the Grafana dashboard\. The name of this role follows this pattern: *workspace\-name*DashboardRole, For instructions on how to create a policy and attach it to a role, see [Modifying a role permissions policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.htmlroles-managingrole-editing-console.html#roles-modify_permissions-policy)\.

   The following example contains the policy to add to the dashboard role\.

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
           "arn:aws:s3:::iottwinmaker-workspace-workspace-name-lower-case-account-id",
           "arn:aws:s3:::iottwinmaker-workspace-workspace-name-lower-case-account-id/*"
         ]
       },
       {
         "Effect": "Allow",
         "Action": [
           "iottwinmaker:Get*",
           "iottwinmaker:List*"
         ],
         "Resource": [
           "arn:aws:iottwinmaker:us-east-1:account-id:workspace/workspace-name",
           "arn:aws:iottwinmaker:us-east-1:account-id:workspace/workspace-name/*"
         ]
       },
       {
         "Effect": "Allow",
         "Action": "iottwinmaker:ListWorkspaces",
         "Resource": "*"
       }
     ]
   }
   ```

You're now ready to start creating a data model for your workspace with your first entity\. For instructions on how to do this, see [Create your first entity](twinmaker-gs-entity.md)\.