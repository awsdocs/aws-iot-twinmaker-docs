--------

--------

# Setting up your Grafana environment<a name="grafana-environment"></a>

You can use Amazon Managed Grafana for a fully managed service, or set up a Grafana environment that you manage yourself\. With Amazon Managed Grafana, you can quickly deploy, operate, and scale open source Grafana for your needs\. Alternatively, you can set up your own infrastructure to manage Grafana servers\.

For more information about both Grafana environment options, see the following topics:
+ [Amazon Managed Grafana](amazon-managed-grafana.md)
+ [Self\-managed Grafana](self-managed-grafana.md)

Also, you'll need to modify CORS configuration of the Amazon S3 bucket for allowing the Grafana user interface to load resources from the bucket. For the instructions, see [CORS Configuration for Grafana scene viewer](cors-configuration-grafana.md)\. 