# Cost optimization

## Introduction

Optimizing costs in environments is a continuous process that requires regular review and adjustment of resources. By analyzing costs, identifying cost reduction opportunities, implementing cost reduction measures, and reviewing their impact, you can ensure that you're not overspending on your environments.

The reference example fictitious company, Proseware, has a target SLO of 99.9% for availability (about 8.7 hours of downtime per year). Its production environment needs SKUs that meet its service level agreements (SLAs), features, and scale needed. But its nonproduction environments don't need the same capabilities. In this section, we will explore how to optimize costs in nonproduction environments by using cheaper SKUs that have lower capacity and SLAs. 

Proseware uses the same infrastructure-as-code (IaC) templates for both the development and production deployments. The only difference is a few SKU variations to optimize costs in the development environment. Proseware chose to use cheaper SKUs in the development environment for Azure Cache for Redis, App Service, and Azure Database for PostgreSQL Flexible Server. The following table shows the services and the SKUs Proseware selected for each environment.

*Table 1. Reference implementation SKU differences between the development and production environments.*

| Service | Development environment SKU | Production environment SKU |
| --- | --- | --- |
| Azure Cache for Redis | Basic | Standard |
| App Service | P1v3 | P2v3 |
| Azure Database for PostgreSQL - Flexible Server | Burstable B1ms (B_Standard_B1ms) | General Purpose D4s_v3 (GP_Standard_D4s_v3) |

### Deploying production SKUs

As this workshop uses a single region, the instructions do not cover deploying production SKUs. However, you can deploy production SKUs by following the instructions in the Reference example by setting the environment ENVIRONMENT to prod. 

```shell
azd env set APP_ENVIRONMENT prod
```

Also, the default deployment is single region.  For multi-region, set LOCATION2 as per instructions. 

```shell
azd env set AZURE_LOCATION2 eastus
```

## Note

Proseware uses Azure Files integrated with App Service to save training videos that users upload.
Consider refactoring this integration to use Azure Storage blobs to reduce hosting costs.

## Next Up

Next, we will explore the concept of reliability in cloud applications. Please proceed to [Part 4 - Reliability](../Part4-Reliability/README.md) for more information.

## Resources
[Well-Architected Framework cost optimization portal](https://learn.microsoft.com/azure/well-architected/cost-optimization)

[Azure Cost Calculator](https://azure.microsoft.com/pricing/calculator)
