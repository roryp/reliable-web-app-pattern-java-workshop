# Reliable Web App Pattern - Java Overview

The reliable web app pattern is a set of principles that helps developers successfully migrate web applications to the cloud. It provides implementation guidance built on the Azure Well-Architected Framework. The pattern focuses on the minimal changes you need to make to ensure the success of your web app in the cloud.

In this workshop we're going to run through the principles and show how to apply them to your web applications.

## Enterprise web app cloud journey

Imagine a web application's evolution in the cloud as a journey. Each step in the journey has different goals. The reliable web app pattern is designed to help you take the first step in the journey. You may only need to "lift and shift" or converge your application to the cloud, and do so with a minimal amount of changes. The reliable web app pattern helps you do that.

![Enterprise web app cloud journey](./images/enterprise-web-app-cloud-journey.png)

Further along in the journey, you may want to refactor your application to take advantage of cloud-native features and then optimize it for scale and performance.

But first, let's explore what it means to converge in the cloud and create an application that's reliable with minimal changes in your existing investment.

## Pattern of patterns

That's not to say that the reliable web app pattern is simplistic. It's not. It's a pattern of patterns. Each pattern provides prescriptive guidance on how to build a specific aspect of a reliable web application. You can use them together or separately.

The image below shows just some of the considerations that you'll need to take into account when converging to the cloud and the reliable web app pattern provides guidance on those. In fact, the reliable web app pattern builds on real-world tested technologies and techniques, like the Azure Well-Architected Framework to produce the set of patterns that you'll learn about in the image below.

![Pattern of patterns](./images/pattern-of-patterns.png)

## Objectives

The objectives of the reliable web app pattern are straightforward. It's designed to help you:

![Objectives](./images/objectives.png)

## The Five Pillars

The reliable web app pattern is built off of 5 pillars. Each pillar is a set of patterns that are derived from both the Azure Well-Architected Framework and 12-factor app methodology.

![The Five Pillars](./images/the-five-pillars.png)

## Summary

Now that you have a basic understanding of what the reliable web app pattern is, let's look at the next 5 sections which give us the reference example to showcase the pillars - we can start on how to optimize costs.

[Part 3 - Cost Optimization](../Part3-Cost-Optimization/README.md)

## Glossary

| Term                           | Description                                                                                           |
|--------------------------------|-------------------------------------------------------------------------------------------------------|
| 12-Factor App                   | A methodology for building software-as-a-service apps that emphasizes portability, scalability, and maintainability. |
| Application Insights           | An Azure service for application performance management and monitoring. |
| Azure AD | Azure Active Directory, Microsoft's cloud-based identity and access management service. |
| Azure App Service | A fully managed platform for building, deploying, and scaling web apps. |
| Azure Cache for Redis          | Managed in-memory data store based on Redis                                                           |
| Azure Database for PostgreSQL  | Managed service for PostgreSQL databases                                                              |
| Azure Files                    | Fully managed file shares in the cloud                                                                |
| Azure Front Door               | Content delivery network and global load balancer                                                     |
| Azure Key Vault                | Centralized storage of application secrets                                                           |
| Azure Monitor | A service that provides full-stack monitoring, advanced analytics, and automated insights across applications and Azure resources. |
| Azure Private Link             | Access to PaaS services over a private endpoint in your virtual network                               |
| Azure Web Application Firewall | Centralized protection of web apps from exploits and vulnerabilities                                 |
| Azure Well-Architected Framework | A guide for building secure, high-performing, resilient, and efficient applications on Azure. |
| Cache-Aside Pattern | A caching pattern where the application code first checks if data is in the cache before retrieving it from the data store. |
| Circuit-Breaker Pattern | A design pattern that stops the application from performing an operation that's likely to fail. |
| Composite SLA                   | Overall availability estimate based on dependencies and architecture                                |
| Dev Containers | A feature in Visual Studio Code that allows you to use a Docker container as a full-featured development environment. |
| Docker | A tool designed to make it easier to create, deploy, and run applications by using containers. |
| Microsoft Entra ID             | Cloud-based identity and access management service                                                    |
| PostgreSQL Flexible Server | A configurable option in Azure for deploying PostgreSQL databases, offering scalability and flexibility. |
| Redis Cache | An in-memory data store used as a database and cache, known for its speed and support for various data structures. |
| Reliable web app pattern       | Principles for updating web apps for the cloud                                                       |
| Retry Pattern | A design pattern that enables an application to retry an operation in the expectation that it'll succeed. |
| Service level agreement (SLA)  | Provider’s commitment to certain service levels                                                       |
| Service level objective (SLO)  | Target for web app availability                                                                       |
| SKU | Stock Keeping Unit, a specific version or configuration of a cloud service or resource. |
| Spring Boot Actuator | A tool to help monitor and manage Spring Boot applications. |
| The Five Pillars | The reliable web app pattern is built off of 5 pillars. Each pillar is a set of patterns that are derived from both the Azure Well-Architected Framework and 12-factor app methodology. |

