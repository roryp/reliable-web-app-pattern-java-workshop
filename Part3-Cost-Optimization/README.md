# Cost optimization

Production environments need SKUs that meet the service level agreements (SLAs), features, and scale needed for production. But nonproduction environments don't normally need the same capabilities. You can optimize costs in nonproduction environments by using cheaper SKUs that have lower capacity and SLAs. 

Proseware uses the same infrastructure-as-code (IaC) templates for development and production deployments. The only difference is a few SKU differences to optimize cost in the development environment. Proseware chose to use cheaper SKUs in the development environment for Azure Cache for Redis, App Service, and Azure Database for PostgreSQL Flexible Server. The following table shows the services and the SKUs Proseware chose for each environment.

*Table 1. Reference implementation SKU differences between the development and production environments.*

| Service | Development environment SKU | Production environment SKU |
| --- | --- | --- |
| Azure Cache for Redis | Basic | Standard |
| App Service | P1v3 | P2v3 |
| Azure Database for PostgreSQL - Flexible Server | Burstable B1ms (B_Standard_B1ms) | General Purpose D4s_v3 (GP_Standard_D4s_v3) |


## Exercise: Analyze Costs

In this exercise, you will Implement Cost Reduction Measures by downgrade the SKU to the Azure nonproduction environment:

```shell
azd env set APP_ENVIRONMENT dev
azd up
```

## Note

Proseware uses Azure Files integrated with App Service to save training videos that users upload. Consider refactoring this integration to use Azure Storage blobs to reduce hosting costs.

## Conclusion

In conclusion, optimizing costs in nonproduction environments is a continuous process that requires regular review and adjustment of resources. By analyzing costs, identifying cost reduction opportunities, implementing cost reduction measures, and reviewing their impact, you can ensure that you're not overspending on your nonproduction environments.

Next, we will explore the concept of reliability in cloud applications. Please proceed to [Part 4 - Reliability](../Part4-Reliability/README.md) for more information.
