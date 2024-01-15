# Security

Welcome to the "Security" section of our workshop. In this part, we will delve into Proseware's Cloud Security Architecture. We'll explore how Proseware leverages advanced cloud technologies and services from Microsoft Azure and Microsoft Entra ID to secure their web applications against various cyber threats, focusing on Managed Identities, Azure App Configuration, and Azure Key Vault.

## Managed Identity

The reference implementation uses Microsoft Entra ID as the identity platform. Using Microsoft Azure AD requires an application registration in the primary tenant. This registration ensures that users who access the web app have identities in the primary tenant. The following Terraform code explicitly enables authentication and requires it to access the web app.

```terraform
data "azuread_client_config" "current" {}

resource "azuread_application" "app_registration" {
  display_name     = "${azurecaf_name.app_service.result}-app"
  owners           = [data.azuread_client_config.current.object_id]
  sign_in_audience = "AzureADMyOrg"  # single tenant
}
```

## Key Vault

Azure Key Vault is a cloud service that safeguards encryption keys and secrets like certificates, connection strings, and passwords. The reference implementation uses private endpoints for Key Vault, Azure Cache for Redis, and Azure Database for PostgreSQL. A private endpoint is a private IP address within a specific virtual network and subnet.

The reference implementation uses the following code to add the Azure Spring Boot Starter For Azure Key Vault Secrets in the `pom.xml`:

```xml
<dependency> 
    <groupId>com.azure.spring</groupId> 
    <artifactId>spring-cloud-azure-starter-keyvault</artifactId> 
</dependency> 
```

### Azure App Configuration

Roles are assigned to users and groups in the Azure Active Directory tenant. The reference implementation creates two app roles (*User* and *Creator*). Roles translate into permissions during authorization. The *Creator* role has permissions to configure the application settings, upload videos, and create playlists. The *User* role can view the videos.

## Exercise - Creating a new *User* 

By default, your user account is added to the application. 
In this exercise, we will explore how to enable additional users.

Let's follow these steps to add a new user to the Proseware application:

- Sign in to the [Azure Portal](https://portal.azure.com).
- Select **Azure Active Directory** -> **Enterprise Applications**.
- Search for, then select **Proseware**.
- Add the user to the application.

![Proseware's Azure Active Directory enterprise applications](./images/AAD-Enterprise-Application.png)

## Conclusion

Next, we will delve into operational excellence in cloud applications --> [Part 6 - Operational Excellence](../Part6-Operational-Excellence/README.md) 

## Resources
[Well-Architected Framework security portal](https://learn.microsoft.com/en-us/azure/well-architected/security)

[Security Checklist](https://learn.microsoft.com/azure/well-architected/security/checklist)
