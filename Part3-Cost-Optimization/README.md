# Cost optimization

Production environments need SKUs that meet the service level agreements (SLAs), features, and scale needed for production. But nonproduction environments don't normally need the same capabilities. You can optimize costs in nonproduction environments by using cheaper SKUs that have lower capacity and SLAs. 

Proseware uses the same infrastructure-as-code (IaC) templates for development and production deployments. The only difference is a few SKU differences to optimize cost in the development environment. Proseware chose to use cheaper SKUs in the development environment for Azure Cache for Redis, App Service, and Azure Database for PostgreSQL Flexible Server. The following table shows the services and the SKUs Proseware chose for each environment. You should choose SKUs that meet the needs of each environment.

```shell
azd env set APP_ENVIRONMENT prod
```

*Table 2. Reference implementation SKU differences between the development and production environments.*

| Service | Development environment SKU | Production environment SKU |
| --- | --- | --- |
| Azure Cache for Redis | Basic | Standard |
| App Service | P1v3 | P2v3 |
| Azure Database for PostgreSQL - Flexible Server | Burstable B1ms (B_Standard_B1ms) | General Purpose D4s_v3 (GP_Standard_D4s_v3) |


## Exercise: Analyze and Reduce Costs

In this exercise, you will analyze the cost of running the reference application in the development and production environments. You will then identify opportunities to reduce costs in the nonproduction environment.

1. **Analyze Costs**: Use the Azure Cost Management and Billing service to analyze the costs associated with each service listed in Table 2. Note the costs for both the development and production environment SKUs.

2. **Identify Cost Reduction Opportunities**: Based on your analysis, identify services where you could potentially reduce costs in the nonproduction environment. Consider downgrading the SKU, reducing the number of instances, or turning off services when they're not in use.

3. **Implement Cost Reduction Measures**: Choose one of the cost reduction opportunities you identified and implement it. For example, you could downgrade the SKU of the Azure Cache for Redis service in the nonproduction environment:

```shell
az redis update --name <your_redis_cache_name> --sku Basic
```

In conclusion, optimizing costs in nonproduction environments is a continuous process that requires regular review and adjustment of resources. By analyzing costs, identifying cost reduction opportunities, implementing cost reduction measures, and reviewing their impact, you can ensure that you're not overspending on your nonproduction environments.

Next, we will explore the concept of reliability in cloud applications. Please proceed to [Part 4 - Reliability](../Part4-Reliability/README.md) for more information.
