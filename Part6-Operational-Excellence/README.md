# Operational Excellence

One of Proseware's business objectives was to reach a 99.9% service level objective for availability. To meet that objective, the web app is deployed to two regions in an active-passive configuration. The active (primary) region handles 100% of user traffic under normal operations and data replicates to the passive (secondary) region asynchronously. This configuration enables Proseware to quickly transition from the primary region to the secondary to mitigate the risk of an outage from impacting their availability.

In this part we will look at using application insights to locate any issues that might arise in the application. We will also look at how to use Azure Monitor to monitor the health of the application.

## Monitoring

The reference implementation demonstrates how to programmatically enable Application Insights. It enable Application Insights, with the following Maven dependency in the `Reference App\src\airsonic-advanced\airsonic-main\pom.xml` file.

```XML
<dependency>
   <groupId>com.microsoft.azure</groupId>
   <artifactId>applicationinsights-runtime-attach</artifactId>
   <version>3.4.7</version>
</dependency>
```

This dependency adds the necessary Application Insights components to your application build. It allows you to visualize metrics in Azure Application Insights. Spring Boot registers several core metrics in Application Insights such as Java virtual machine (JVM), CPU, Tomcat, and others. Application Insights automatically collects from logging frameworks such as Log4j and Logback.

## Application Map

The Application Map in Application Insights provides a visual representation of the components in your application and the relationships between them. It automatically discovers application components and maps the communication between them. The Application Map is useful for identifying performance bottlenecks and failures in your application.

1. Select **Application Map** from the left-hand menu.
2. Select the **Web** component to view the Application Map for your application.

In Part 4 we triggered a failure. and the exceptions from Part 4 in the Prosware web application were reported with Application Insights. as you recall, some videos in Proseware cannot be played correctly. 

![VideoError](images/proseware-video-error.png)

Open *Application Insights*, and fine the `NullPointerException` that was thrown while processing the videos.

![AppInsightsFailures](images/application-insights-failures.png)
![AppInsightsEndToEndDetails](images/application-insights-end-to-end-details.png)

## Conclusion

Combined with Part 4, maintaining operational excellence in your cloud application involves a combination of monitoring, diagnostics, and recovery strategies. By leveraging Azure services, you can ensure your application remains robust and resilient under varying operational conditions.

Next, we will explore performance efficiency in cloud applications. Please proceed to [Part 7 - Performance Efficiency](../Part7-Performance-Efficiency/README.md) for more information.
