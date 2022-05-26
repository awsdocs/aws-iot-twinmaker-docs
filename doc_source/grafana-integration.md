--------

--------

# AWS IoT TwinMaker Grafana dashboard integration<a name="grafana-integration"></a>

AWS IoT TwinMaker supports Grafana integration through an application plugin\. You can use Grafana version 8\.2\.0 and later to interact with your digital twin application\. The AWS IoT TwinMaker plugin provides custom panels, dashboard templates, and a datasource to connect to your digital twin data\.

For more information about how to onboard with Grafana and set up permissions for your dashboard, see the following topics:
+ [Setting up your Grafana environment](grafana-environment.md)
+ [Creating a dashboard IAM role](dashboard-IAM-role.md)
+ [Creating an AWS IoT TwinMaker video player policy](tm-video-policy.md)

For more information about the AWS IoT TwinMaker Grafana plugin, see the [AWS IoT TwinMaker App](https://grafana.com/grafana/plugins/grafana-iot-twinmaker-app/) documentation\.

For more information about the key components of the Grafana plugin, see the following:
+ [AWS IoT TwinMaker datasource](https://github.com/grafana/grafana-iot-twinmaker-app/blob/main/src/datasource/README.md)
+ [Dashboard templates](https://github.com/grafana/grafana-iot-twinmaker-app/blob/main/src/datasource/dashboards/README.md)
+ [Scene Viewer panel](https://github.com/grafana/grafana-iot-twinmaker-app/blob/main/src/panels/scene-viewer/README.md)
+ [Video Player panel](https://github.com/grafana/grafana-iot-twinmaker-app/blob/main/src/panels/video-player/README.md)