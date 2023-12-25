# Cost optimization

## Introduction

The reference example fictitious company, Proseware, has a target SLO of 99.9% for availability (about 8.7 hours of downtime per year). Its production environment needs SKUs that meet its service level agreements (SLAs), features, and scale needed. But its nonproduction environments don't need the same capabilities. In this section, we will explore how to optimize costs in nonproduction environments by using cheaper SKUs that have lower capacity and SLAs. 

Proseware uses the same infrastructure-as-code (IaC) templates for both the development and production deployments. The only difference is a few SKU variations to optimize costs in the development environment. Proseware chose to use cheaper SKUs in the development environment for Azure Cache for Redis, App Service, and Azure Database for PostgreSQL Flexible Server. The following table shows the services and the SKUs Proseware selected for each environment.

*Table 1. Reference implementation SKU differences between the development and production environments.*

| Service | Development environment SKU | Production environment SKU |
| --- | --- | --- |
| Azure Cache for Redis | Basic | Standard |
| App Service | P1v3 | P2v3 |
| Azure Database for PostgreSQL - Flexible Server | Burstable B1ms (B_Standard_B1ms) | General Purpose D4s_v3 (GP_Standard_D4s_v3) |


## Exercise - Implementing Cost Reduction Measures

In this exercise, we will focus on implementing cost reduction measures for our Azure environment. One effective way to manage costs in Azure is by adjusting the Service Tier or SKU (Stock Keeping Unit) based on the environment's requirements. 

For non-production environments like development or testing, we can often use a lower SKU as these environments typically don't require the same performance or capacity as production environments. This approach can significantly reduce costs.

We will be downgrading the SKU for our non-production environment using Azure Developer CLI  (azd) commands. 

Here are the steps:
Before we start, you need to open a shell window and log into Azure Dev CLI (azd) and Azure CLI. Here's how:

1. Open a shell window.
2. Authenticated to Azure and have the appropriate subscription selected.  To authenticate:

```shell
az login --scope https://graph.microsoft.com//.default
azd auth login
```

Now that you're logged into Azure CLI and Azure Developer CLI, you can proceed with the steps to downgrade the SKU for your non-production environment.

1. Set the application environment to 'dev' using the following command:

    ```shell
    azd env set APP_ENVIRONMENT dev
    ```

2. Apply the changes using the `azd up` command:

    ```shell
    azd up
    ```

## Note

Proseware uses Azure Files integrated with App Service to save training videos that users upload.
Consider refactoring this integration to use Azure Storage blobs to reduce hosting costs.

## Conclusion

Optimizing costs in nonproduction environments is a continuous process that requires regular review and adjustment of resources. By analyzing costs, identifying cost reduction opportunities, implementing cost reduction measures, and reviewing their impact, you can ensure that you're not overspending on your nonproduction environments.

Next, we will explore the concept of reliability in cloud applications. Please proceed to [Part 4 - Reliability](../Part4-Reliability/README.md) for more information.

## Resources
[Well-Architected Framework cost optimization portal](https://learn.microsoft.com/azure/well-architected/cost-optimization)
[Azure Cost Calculator](https://azure.microsoft.com/pricing/calculator)
