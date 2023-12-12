## Cost optimization

Cost optimization principles balance business goals with budget justification to create a cost-effective web application. They help reduce unnecessary expenses and improving operational efficiencies. This section describes cost-optimization recommendations to apply while re-platforming a web app.

*Reference implementation.* The app uses Azure Files integrated with App Service to save training videos that users upload. Refactoring this integration to use Azure Storage blobs would reduce hosting costs and should be evaluated as a possible future modernization.

### Rightsize resources for each environment

Production environments need SKUs that meet the service level agreements (SLAs), features, and scale needed for production. But nonproduction environments don't normally need the same capabilities. You can optimize costs in nonproduction environments by using cheaper SKUs that have lower capacity and SLAs. You should consider Azure Dev/Test pricing and Azure Reservations. How and whether you use these cost-saving methods depends on your environment.

**Consider Azure Dev/Test pricing.** Azure Dev/Test pricing gives you access to select Azure services for nonproduction environments at discounted pricing under the Microsoft Customer Agreement. The plan reduces the costs of running and managing applications in development and testing environments, across a range of Microsoft products. For more information, see [Dev/Test pricing options](https://azure.microsoft.com/pricing/dev-test/#overview).

*Reference implementation.* This architecture doesn't apply Azure Dev/Test pricing. Azure Dev/Test pricing didn't cover any of the components.

**Consider using cheaper SKUs in non-production environments.** You can use different SKUs across environments to save cost. If you use different SKUs or components for development, you might not encounter specific application issues until you deploy to production. It's essential to account for these differences and incorporate them into your testing cycles. For instance, if you only use Web Application Firewall (WAF) and Azure Front Door in production, you might not discover potential WAF false positives (valid requests that WAF blocks), routing problems, and host-header issues until you deploy the application to production.

*Reference implementation.* The reference implementation has an optional parameter that deploys different SKUs. An environment parameter instructs the Terraform template to select development SKUs. The following code shows this environment parameter.

```shell
azd env set APP_ENVIRONMENT prod
```

Proseware uses the same infrastructure-as-code (IaC) templates for development and production deployments. The only difference is a few SKU differences to optimize cost in the development environment. Proseware chose to use cheaper SKUs in the development environment for Azure Cache for Redis, App Service, and Azure Database for PostgreSQL Flexible Server. The following table shows the services and the SKUs Proseware chose for each environment. You should choose SKUs that meet the needs of each environment.

*Table 2. Reference implementation SKU differences between the development and production environments.*

| Service | Development environment SKU | Production environment SKU |
| --- | --- | --- |
| Azure Cache for Redis | Basic | Standard |
| App Service | P1v3 | P2v3 |
| Azure Database for PostgreSQL - Flexible Server | Burstable B1ms (B_Standard_B1ms) | General Purpose D4s_v3 (GP_Standard_D4s_v3) |

**Consider Azure Reservations or an Azure savings plan.** You can combine an Azure savings plan with Azure Reservations to optimize compute cost and flexibility. Azure Reservations helps you save by committing to one-year or three-year plans for multiple products. The Azure savings plan for compute is the most flexible savings plan. It generates savings on pay-as-you-go prices. Pick a one-year or three-year commitment for compute services, regardless of region, instance size, or operating system. Eligible compute services include virtual machines, dedicated hosts, container instances, Azure Functions Premium, and App Service.

Plan your commitments around your team's architecture roadmap. For example, if you plan on being using the same database engine for a year or more, that would make a good candidate for a reserved instance. For more information, see [Azure Reservations](https://learn.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations) and [Azure savings plans for compute](https://learn.microsoft.com/azure/cost-management-billing/savings-plan/savings-plan-compute-overview).

*Reference implementation.* Azure Database for PostgreSQL is a prime candidate for a reserved instance based on the plan to stick with this database engine for at least a year after this initial convergence on the cloud phase.

### Automate scaling the environment

You should use autoscale to automate horizontal scaling for production environments. Autoscaling adapts to user demand to save you money. Horizontal scaling automatically increases compute capacity to meet user demand and decreases compute capacity when demand drops. Don't increase the size of your application platform (vertical scaling) to meet frequent changes in demand. It's less cost efficient. For more information, see [Scale up an app in Azure App Service](https://learn.microsoft.com/azure/app-service/manage-scale-up) and [Overview of autoscale in Microsoft Azure](https://learn.microsoft.com/azure/azure-monitor/autoscale/autoscale-overview).

### Delete nonproduction environments

To optimize cost, it's recommended that you delete nonproduction environments during periods of low activity such as business hours or holidays. Additionally, it's important to ensure that any unused environments are deleted in a controlled and repeatable process. For example, you can build a deployment pipeline with automated steps for deleting environments.