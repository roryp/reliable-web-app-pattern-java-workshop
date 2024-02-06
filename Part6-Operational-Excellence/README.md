# Operational Excellence

Proseware aims for a 99.9% service level objective to ensure reliability and availability. Monitoring and proactive maintenance are crucial for anticipating issues and improving customer experience. 
Operational excellence in cloud applications involves monitoring, diagnostics, and recovery strategies. Azure provides many tools to help ensure application robustness and resilience, allowing quick transition to a secondary region to mitigate outages.

## Exercise - Application Map

In Part 4, the player service couldn't access the stored files due to the circuit breaker being open.
In this exercise, we'll use Application Map to locate the NullPointerExceptions:

1. Select **Application Map** from the left-hand menu.
2. Select the **Web** component to view the Application Map for your application.

Open *Application Insights*, and find the `NullPointerException` that was thrown while processing the videos.

![AppInsightsFailures](images/application-insights-failures.png)

You can also select the transactions on the right-hand side to see the end-to-end details of the failed requests.

![AppInsightsEndToEndDetails](images/application-insights-end-to-end-details.png)

## Next Up

Next, we will explore performance efficiency in cloud applications-- > [Part 7 - Performance Efficiency](../Part7-Performance-Efficiency/README.md)

## Resources
[Well-Architeched Framework operational portal](https://learn.microsoft.com/en-us/azure/well-architected/operational-excellence)
