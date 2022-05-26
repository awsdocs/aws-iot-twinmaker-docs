--------

--------

# Before you create your first scene<a name="scenes-before-starting"></a>

 Scenes rely on resources to represent your digital twin\. These resources are made up of 3D models, data, or texture files\. The size and complexity of your resources, elements in the scene such as lighting, and your computer hardware, impact the performance of AWS IoT TwinMaker scenes\. Use the information in this topic to reduce lag, loading times, and improve the frame rate of your scenes\.

## Optimize your resources before importing them into AWS IoT TwinMaker<a name="scenes-before-starting-3D-optimization"></a>

You can use AWS IoT TwinMaker to interact with your digital twin in real time\. For the best experience with your scenes, we recommend optimizing your resources for use in a real\-time environment\.

Your 3D models can have a significant impact on performance\. Complex model geometry and meshes can reduce performance\. For example, industrial CAD models have a high level of detail\. We recommend compressing these model's meshes and reducing their polygon count before using them in AWS IoT TwinMaker scenes\. If you're creating new 3D models for AWS IoT TwinMaker, you should establish a level of detail and maintain it across all your models\. Remove details from models that don’t affect the visualization or interpretation of your use case\.\.

To compress models and reduce the file size, use open source mesh compression tools, such as [DRACO 3D data compression](https://google.github.io/draco/)\.

Unoptimized textures can also impact performance\. If you don’t require any transparency in your textures, considering choosing the PEG image format over the PNG format\. You can compress your texture files by using open source texture compression tools, such as [Basis Universal texture compression](https://www.khronos.org/blog/google-and-binomial-contribute-basis-universal-texture-format-to-khronos-gltf-3d-transmission-open-standard)\.

## Best practices for performance in AWS IoT TwinMaker<a name="scenes-best-practices-optimization"></a>

For the best performance with AWS IoT TwinMaker, note the following limitations and best practices\.
+ AWS IoT TwinMaker scene rendering performance is hardware dependent\. Performance varies across different computer hardware configurations\.
+ We recommend a total polygon count of under 1 million across all your objects in your AWS IoT TwinMaker\.
+ We recommend a total of 200 objects per scene\. Increasing the number of objects in a scene beyond 200 can decrease your scene frame rate\.
+ Total scene size should not exceed 50 megabytes\.
+ Scenes have ambient lighting by default\. You can add extra lights into a scene to bring certain objects into focus, or cast shadows on objects\. We recommend using one light per scene\. Use lights where needed, and avoid replicating real\-world lights within a scene\.

## Learn more<a name="scenes-learn-more"></a>

Use these resources to learn more about optimization techniques that you can use to improve performance in your scenes\.
+ [How to convert CAD assets to glTF for use with AWS IoT TwinMaker](https://aws.amazon.com/blogs/iot/how-to-convert-cad-assets-to-gltf-for-aws-iot-twinmaker/)
+ [Optimize your 3D models for web content](https://medium.com/@michael.andrew/6-things-you-havent-optimised-in-your-webvr-content-272d74d541f0)
+ [Optimizing scenes for better WebGL performance](https://www.soft8soft.com/docs/manual/en/introduction/Optimizing-WebGL-performance.html)