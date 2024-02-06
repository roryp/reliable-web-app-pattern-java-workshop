# Reliable Web App Pattern - Java Overview

The reliable web app pattern is a set of principles that helps developers successfully migrate web applications to the cloud. The pattern focuses on the minimal changes you need to make to ensure the success of your web app in the cloud.

![Pattern of patterns](./images/pattern-of-patterns.png)

## Objectives

The objectives of the reliable web app pattern are straightforward. It's designed to help you:

![Objectives](./images/objectives.png)

## The Five Pillars

The reliable web app pattern is built off of 5 pillars. Each pillar is a set of patterns that are derived from both the Azure Well-Architected Framework and 12-factor app methodology.

![The Five Pillars](./images/the-five-pillars.png)

## Reference Application

The reference implementation provides a production-grade web application based on these patterns for developers to build their own reliable web application in Azure.

![Reference Application](./images/reliable-web-app-java.svg)

- The reference application has the option to use either one or two regions in an active-passive configuration to meet the service level objective of 99.9%. The two region option uses **Azure Front Door** as the global load balancer. Front Door routes all traffic to the active region. The passive region is for failover only. The failover plan is manual and there are no automated scripts with this repo.
- All inbound HTTPS traffic passes through Front Door and **Web Application Firewall (WAF)**. WAF inspects the traffic against WAF policies.
- **Azure App Service** uses **Azure Virtual Network** integration to communicate securely with other Azure resources within the private virtual network. App Service requires an `App Service delegated subnet` in the virtual network to enable virtual network integration.
- **Azure Key Vault** and **Azure Cache for Redis** have private endpoints in the `Private endpoints subnet`. Private DNS zones linked to the virtual network resolve DNS queries for these Azure resources to their private endpoint IP address.
- **Azure Database for PostgreSQL - Flexible Server** uses virtual network integration for private communication. It doesn't support private endpoints.
- The web app uses an account access key to mount a directory with **Azure Files** to the App Service. A private endpoint is not used for Azure Files to facilitate the deployment of the reference implementation for everyone. However, it is recommended to use a private endpoint in production as it adds an extra layer of security. Azure Files only accepts traffic from the virtual network and the local client IP address of the user executing the deployment.
- The web app code implements the Retry, Circuit Breaker, and Cache-Aside patterns. The web app integrates with **Azure Entra** using the Spring Boot Starter for Azure Active Entra.
- **Azure Application Insights** is the application performance management tool, and it gathers telemetry data on the web app.
- App Service, Azure Files, Key Vault, Azure Cache for Redis, and Azure Database for PostgreSQL use diagnostic settings to send logs and metrics to **Azure Log Analytics Workspace**. Log Analytics Workspace is used to monitor the health of Azure services.
- Azure Database for PostgreSQL uses a high-availability zone redundant configuration and a read replica in the passive region for failover.

## Next Up

The next five sections will delve into each pillar. We will begin by examining cost optimization strategies.

[Part 3 - Cost Optimization](../Part3-Cost-Optimization/README.md)


