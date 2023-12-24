# Reliability

In this part, we'll guide you through the process of testing and configuring two key code-level design patterns: retry, and circuit-breaker, using our implementation. The upcoming sections will provide detailed steps for you to follow and experiment with these design patterns.

## Retry and Circuit Break Pattern

We built an app configuration setting that lets you simulate and test a transient failure when making a web request to GitHub. The reference implementation uses the `Spring Boot Actuator` to monitor retries. After deploying the application, navigate to your site’s `/actuator` endpoint to see a list of Spring Boot Actuator endpoints. Navigate to `/actuator/retries` to see retried calls. Set the `AIRSONIC_RETRY_DEMO` application setting to 1. This will simulate a failure for every web request to GitHub. A value of 2 generates a 503 error for every other request.

![airsonic-retry-demo](images/airsonic-retry-demo.png)

Follow these steps to set up this test:

1. Set the `AIRSONIC_RETRY_DEMO` setting to 1 in App Service Configuration.

1. Changing a application setting will cause the App Service to restart. Wait for the app to restart.

1. We added Spring Actuator Dependencies to the Proseware project. This enables actuator endpoints. Navigate to the following sites.
    * https://<APP_NAME>.azurewebsites.net/actuator
    * https://<APP_NAME>.azurewebsites.net/actuator/retryevents
    * https://<APP_NAME>.azurewebsites.net/actuator/retryevents
    * https://<APP_NAME>.azurewebsites.net/actuator/metrics/resilience4j.circuitbreaker.not.permitted.calls

1. Navigate to https://<APP_NAME>.azurewebsites.net/index and refresh the page. Every time you refresh the page, a call to GitHub is made.

1. Make note of the retry events and circuit breaker in the actuator endpoints.

![airsonic-retry-demo](images/proseware-retries.png)

![airsonic-retry-demo](images/proseware-circuit-breaker.png)

## Logs

Application logging is enabled. To view the logs, navigate to *Diagnose and solve problems*. From there, click on *Application Logs*.

![Diagnose and solve problems](images/appservice-diagnose-and-solve-problems.png)

![Application Logs](images/appservice-diagnose-and-solve-problems-application-logs.png)

## Summary

In this part, we learned how to test and configure two key code-level design patterns: retry, and circuit-breaker, using our implementation. We also learned how to view application logs.

In the sixth part of our series, we will delve into the use of Azure Monitor, a comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments. This powerful tool will aid us in troubleshooting any issues that may arise. Following that, in Part 7, we will shift our focus to the utilization of Redis Cache. Redis Cache is an in-memory data structure store, used as a database, cache, and message broker. We will explore its performance efficiency and how it can be leveraged to enhance the speed and responsiveness of our applications

Please proceed to [Part 5 - Security](../Part5-Security/README.md) for more information.
