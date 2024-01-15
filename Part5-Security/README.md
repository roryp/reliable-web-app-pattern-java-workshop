# Security

Welcome to the "Security" section of our workshop. In this part, we will delve into Proseware's Cloud Security Architecture. We'll explore how Proseware leverages advanced cloud technologies and services from Microsoft Azure and Microsoft Entra ID to secure their web applications against various cyber threats.


## Review in the Reliable Web App's Code

Let's review where how Managed Identity, Azure App Configuration, and Azure Key Vault are used in the Reliable Web App reference application.

The reference implementation uses Microsoft Entra ID as the identity platform. Using Microsoft Entra ID as the identity platform requires an application registration in the primary tenant. The application registration ensures the users that get access to the web app have identities in the primary tenant. The following Terraform code explicitly enables authentication and requires authentication to access the web app.

```terraform
data "azuread_client_config" "current" {}

resource "azuread_application" "app_registration" {
  display_name     = "${azurecaf_name.app_service.result}-app"
  owners           = [data.azuread_client_config.current.object_id]
  sign_in_audience = "AzureADMyOrg"  # single tenant
}
```

The following code configures Microsoft Entra ID as the authentication provider. It uses a client secret stored in an Azure Key Vault.

```java
spring.cloud.azure.active-directory.enabled=true
spring.cloud.azure.active-directory.credential.client-id=
spring.cloud.azure.active-directory.profile.tenant-id=
spring.cloud.azure.active-directory.credential.client-secret=${airsonic-application-client-secret}
spring.cloud.azure.active-directory.authorization-clients.graph.scopes=https://graph.microsoft.com/User.Read
```

**Integrate with the identity provider.** You need to integrate the web application with the identity provider (Microsoft Entra ID) in the code to help ensure secure and seamless authentication and authorization.

The Spring Boot Starter for Microsoft Entra ID is an excellent option for integrating with Microsoft Entra ID. This starter provides a simple and efficient way to implement enhanced-security authentication at the code level. It uses the Spring Security and Spring Boot frameworks. The Spring Boot Starter for Microsoft Entra ID provides several benefits. It supports various authentication flows, automatic token management, and customizable authorization policies. It enables integration with other Spring Cloud components such as Spring Cloud Config and Spring Cloud Gateway. By using the Spring Boot Starter for Microsoft Entra ID, you can integrate Microsoft Entra ID and OAuth 2.0 authentication and authorization into the Spring Boot application without manually configuring the required libraries and settings. For more information, see [Spring Boot Starter for Microsoft Entra ID](/azure/developer/java/spring-framework/spring-boot-starter-for-azure-active-directory-developer-guide?tabs=SpringCloudAzure4x).

*Reference implementation.* The reference implementation uses the Microsoft identity platform (Microsoft Entra ID) as the identity provider for the web app. It uses the OAuth 2.0 authorization code grant to sign in a user with a Microsoft Entra account. The following XML snippet defines the two required dependencies of the OAuth 2.0 authorization code grant flow. The dependency `com.azure.spring: spring-cloud-azure-starter-active-directory` enables Microsoft Entra authentication and authorization in a Spring Boot application. The dependency `org.springframework.boot: spring-boot-starter-oauth2-client` supports OAuth 2.0 authentication and authorization in a Spring Boot application.

```xml
<dependency>
    <groupId>com.azure.spring</groupId>
    <artifactId>spring-cloud-azure-starter-active-directory</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

**Implement authentication and authorization business rules.** Implementing authentication and authorization business rules involves defining the access control policies and permissions for various application functionalities and resources. You need to configure Spring Security to use Spring Boot Starter for Microsoft Entra ID. This library allows integration with Microsoft Entra ID and helps you ensure that users are authenticated securely. Configuring and enabling the Microsoft Authentication Library (MSAL) provides access to more security features. These features include token caching and automatic token refreshing.

*Reference implementation* The reference implementation creates two app roles (*User* and *Creator*). Roles translate into permissions during authorization. The *Creator* role has permissions to configure the application settings, upload videos, and create playlists. The *User* role can view the videos.

To integrate with Microsoft Entra ID, the reference implementation had to refactor the [`GlobalSecurityConfig.java`](https://github.com/Azure/reliable-web-app-pattern-java/blob/main/src/airsonic-advanced/airsonic-main/src/main/java/org/airsonic/player/security/GlobalSecurityConfig.java). `GlobalSecurityConfig.java` has the class-level annotation `@EnableWebSecurity`. `@EnableWebSecurity` enables Spring Security to locate the class and allows the class to have custom Spring Security configuration defined in any `WebSecurityConfigurer`. `WebSecurityConfigurerAdapter` is the implementation class of the `WebSecurityConfigurer` interface. Extending the `WebSecurityConfigurerAdapter` class enables endpoint authorization.

For Microsoft Entra ID, the [`AadWebSecurityConfigurerAdapter`](https://github.com/Azure/azure-sdk-for-java/blob/main/sdk/spring/spring-cloud-azure-autoconfigure/src/main/java/com/azure/spring/cloud/autoconfigure/aad/AadWebSecurityConfigurerAdapter.java) class protects the routes in a Spring application, and it extends `WebSecurityConfigurerAdapter`. To configure the specific requirements for the reference implementation, the `WebSecurityConfiguration` class in the following code extends `ADWebSecurityConfigurationAdapter`.

The `antMatchers` method enforces authorization to the specified routes. For example, users making a request to `/deletePlaylist*` must have the role `APPROLE_Creator`. The code doesn't allow users without `APPROLE_Creator` to make the request.

```java
@Configuration
public class WebSecurityConfiguration extends AadWebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
      // Use required configuration from AadWebSecurityAdapter.configure:
      super.configure(http);
      // Add custom configuration:

      http
          .authorizeRequests()
          .antMatchers("/recover*", "/accessDenied*", "/style/**", "/icons/**", "/flash/**", "/script/**", "/error")
          .permitAll()
          .antMatchers("/deletePlaylist*", "/savePlaylist*")
          .hasAnyAuthority("APPROLE_Creator")
          .antMatchers("/**")
          .hasAnyAuthority("APPROLE_User", "APPROLE_Creator")
          .anyRequest().authenticated()
          .and()
          .addFilterBefore(aadAddAuthorizedUsersFilter UsernamePasswordAuthenticationFilter.class)
          .logout(logout -> logout
              .deleteCookies("JSESSIONID", "XSRF-TOKEN")
              .clearAuthentication(true)
              .invalidateHttpSession(true)
              .logoutSuccessUrl("/index"));
    }
    ... 
}
```

**Express your application needs in Azure AD.** Most apps use application roles. *Application roles* are custom roles for assigning permissions to users or applications. The application code defines the application roles, and it interprets the application roles as permissions during authorization. You can define application roles as Microsoft Entra roles that the MSAL configuration can use. The Microsoft Entra roles provide the backing for the access that the application roles receive. Microsoft Entra authorizes users by using the application roles.

The `appRoles` attribute in Microsoft Entra ID defines the roles that an app can declare in the application manifest. The `appRoles` attribute allows applications to define their own roles. When a user signs in to the application, Microsoft Entra ID generates an ID token that contains various claims. This token includes a roles claim that lists the roles assigned to the user.

*Reference implementation.* The reference implementation uses an app registration to assign Microsoft Entra users an app role (*User* or *Creator*). The app roles allow users to sign in to the application. The following JSON shows what the *User* and *Creator* `appRoles` look like in Terraform.

```terraform
data "azuread_client_config" "current" {}

resource "azuread_application" "app_registration" {
  display_name     = "${azurecaf_name.app_service.result}-app"
  owners           = [data.azuread_client_config.current.object_id]
  sign_in_audience = "AzureADMyOrg"  # single tenant

  app_role {
    allowed_member_types = ["User"]
    description          = "ReadOnly roles have limited query access"
    display_name         = "ReadOnly"
    enabled              = true
    id                   = random_uuid.user_role_id.result
    value                = "User"
  }

  app_role {
    allowed_member_types = ["User"]
    description          = "Creator roles allows users to create content"
    display_name         = "Creator"
    enabled              = true
    id                   = random_uuid.creator_role_id.result
    value                = "Creator"
  }
}
```

### Configure service authentication and authorization

You need to configure user authentication and authorization so users can access the web app. You also need to configure service authentication and authorization so the services in your environment have the permissions to perform necessary functions.

**Use managed identities.** Managed identities create an identity in Microsoft Entra ID that eliminates the need for developers to manage credentials. The web app receives a workload identity (service principal) in Microsoft Entra ID. Azure manages the access tokens behind the scenes. Managed identities provide benefits for authentication, authorization, and accounting. For example, you can use a managed identity to grant the web app access to other Azure resources such as Azure Key Vault and Azure databases. You can also use a managed identity to enable a CI/CD pipeline that deploys a web app to App Service.

However, keeping your on-premises authentication and authorization configuration can improve your migration experience in some cases. For example, hybrid deployments, legacy systems, and robust on-premises identity solutions could be reasons to delay the adoption of managed identities. You should keep the on-premises setup and modernize your identity solution later.

*Reference implementation.* The reference implementation keeps the on-premises authentication mechanism for the database (username and password). As a result, the reference implementation stores the database secret in Key Vault. The web app uses a managed identity (system assigned) to retrieve secrets from Key Vault.

### Use a central secrets store (Key Vault)

The term *secret* refers to anything that you don't want exposed in plain text (passwords, keys, certificates). After you migrate your app to the cloud, you might have secrets that you need to manage. You should store all these secrets in Key Vault.

Many on-premises environments don't have a central secrets store. As a result, key rotation is uncommon and auditing access to a secret is difficult. In Azure, the central secrets store is Key Vault. You can use Key Vault to store keys and to manage, audit, and monitor access to secrets.

*Reference implementation.* The reference implementation stores the following secrets in Key Vault: (1) PostgreSQL database username and password, (2) Redis Cache password, and (3) the client secret for Microsoft Entra ID associated with the MSAL implementation.

**Don't put Key Vault in the HTTP-request flow.** Key Vault has service limitations to safeguard resources and ensure optimal service quality for its clients. The original intent of Key Vault was to store and retrieve sensitive information during deployment. Organizations sometimes use Key Vault for runtime secret management, and many applications and services treat it like a database. However, the Key Vault limitations don't support high throughput rates and might affect performance if Key Vault is in the HTTP-request flow. When a key vault reaches a service threshold, it limits any further requests from the client and returns HTTP status code 429. The web app should load values from Key Vault at application start time. For more information, see [Key Vault transaction limits](/azure/key-vault/general/service-limits#secrets-managed-storage-account-keys-and-vault-transactions).

**Use one method to access secrets in Key Vault.** There are two methods to configure a web app to access secrets in Key Vault. (1) You can use an app setting in App Service and inject the secret as an [environment variable](/azure/app-service/app-service-key-vault-references#azure-resource-manager-deployment). (2) You can reference the secret in your application code. Add a reference to the app properties file so the app can communicate with Key Vault. You should pick one of these two methods and use it consistently. You should also avoid using both methods because it creates unneeded complexity.  

To integrate Key Vault with a Spring application, you need to (1) add the Azure Spring Boot Starter For Azure Key Vault Secrets in the `pom.xml` file and (2) configure a Key Vault endpoint in either the `application.properties` file or as an environment variable.

*Reference implementation.* The reference implementation uses the following code to add the Azure Spring Boot Starter For Azure Key Vault Secrets in the `pom.xml`:

```xml
<dependency> 
    <groupId>com.azure.spring</groupId> 
    <artifactId>spring-cloud-azure-starter-keyvault</artifactId> 
</dependency> 
```

The reference implementation uses an environment variable in the [App Service Terraform](https://github.com/Azure/reliable-web-app-pattern-java/blob/main/terraform/modules/app-service/main.tf) file to configure the Key Vault endpoint. The following code shows the environment variable.

```terraform
SPRING_CLOUD_AZURE_KEYVAULT_SECRET_PROPERTY_SOURCES_0_ENDPOINT=var.key_vault_uri
```

The reference implementation sets the property `spring.cloud.azure.keyvault.secret.property-source-enabled` to `true` in the `application.properties` file. This property allows Spring Cloud Azure to inject secrets from Azure Key Vault. The `${database-app-user-password}` is an example of Spring Cloud Azure injecting a secret into the web application.

```java
spring.cloud.azure.keyvault.secret.property-source-enabled=true

spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=
spring.datasource.username=${database-app-user}
spring.datasource.password=${database-app-user-password}
```

**Avoid using access keys for temporary access where possible.** Granting permanent access to a storage account is a security risk. If attackers obtain the access keys, they have permanent access to your data. It's a best practice to use temporary permissions to grant access to resources. Temporary permissions reduce the risk of unauthorized access or data breaches.

For temporary account access, you should use a shared access signature (SAS). There's a user delegation SAS, a service SAS, and an account SAS. You should use a user delegation SAS when possible. It's the only SAS that uses Microsoft Entra credentials and doesn't require a storage account key.

*Reference implementation.* Sometimes access keys are unavoidable. The reference implementation needs to use a [Storage account access key](/azure/storage/common/storage-account-keys-manage) to mount a directory with Azure Files to App Service. The web app uses the Azure Files integration in App Service to mount an NFS share to the Tomcat app server. The mount allows the web app to access the file share as if it were a local directory. This setup enables the web app to read and write files to the shared file system in the cloud.

### Use private endpoints

Private endpoints provide private connections between resources in an Azure virtual network and Azure services. By default, communication to most Azure services crosses the public internet. You should use private endpoints in all production environments for all supported Azure services. Private endpoints don't require any code changes, app configurations, or connection strings. For more information, see [How to create a private endpoint](/azure/architecture/example-scenario/private-web-app/private-web-app#deploy-this-scenario) and [Best practices for endpoint security](/azure/architecture/framework/security/design-network-endpoints).

*Reference implementation.* The reference implementation uses private endpoints for Key Vault, Azure Cache for Redis, and Azure Database for PostgreSQL. The reference implementation doesn't use a private endpoint for Azure Files for deployment purposes. The web app needs to load the user interface with playlists and videos from the local client IP address. A private endpoint would block this deployment step. So we opted for a service firewall. Azure Files only accepts traffic from the virtual network and the local client IP of the user executing the deployment. Since you don't need to populate data like this in production, you should use a private endpoint.

### Use a web application firewall

You should protect web applications with a web application firewall. The web application firewall provides a level protection against common security attacks and botnets. To take full advantage of the web application firewall, you must prevent traffic from bypassing it.

You should restrict access on the application platform (App Service) to accept only inbound communication from your gateway instance, Azure Front Door in this architecture. You can (1) [use Azure Front Door private endpoint](/azure/frontdoor/private-link), or (2) you can filter requests by the `X-Azure-FDID` header value. The App Service platform and Java Spring can filter by header value. You should use App Service as the first option. Filtering at the platform level prevents unwanted requests from reaching your code. You need to configure what traffic you want to pass through your WAF. You can filter based on the host name, client IP, and other values. For more information, see [Preserve the original HTTP host name](/azure/architecture/best-practices/host-name-preservation)

*Reference implementation.* The reference implementation filters requests to ensure they pass through the WAF. It uses a native network control in App Service that looks for a specific `X-Azure-FDID` value.

```terraform
resource "azurerm_linux_web_app" "application" {

    site_config {

        ip_restriction {
           service_tag               = "AzureFrontDoor.Backend"
          ip_address                = null
          virtual_network_subnet_id = null
          action                    = "Allow"
          priority                  = 100
          headers {
            x_azure_fdid      = [var.frontdoor_profile_uuid]
            x_fd_health_probe = []
            x_forwarded_for   = []
            x_forwarded_host  = []
          }
          name = "Allow traffic from Front Door"
        }
    }
}
```

### Configure database security

Administrator-level access to the database grants permissions to perform privileged operations. Privileged operations include creating and deleting databases, modifying table schemas, or changing user permissions. Developers often need administrator-level access to maintain the database or troubleshoot issues.

**Avoid permanent elevated permissions.** You should only grant the developers just-in-time access to perform privileged operations. With just-in-time access, users receive temporary permissions to perform privileged tasks

**Don't give application elevated permissions.** You shouldn't grant administrator-level access to the application identity. You should configure least-privileged access for the application to the database. It limits the blast radius of bugs and security breaches. You have two primary methods to access the Azure PostgreSQL database. You can use Microsoft Entra authentication or PostgreSQL authentication. For more information, see [JDBC with Azure PostgreSQL](/azure/developer/java/spring-framework/configure-spring-data-jdbc-with-azure-postgresql).


### Private endpoints

Azure KeyVault and Azure App Configuration are not accessible from the public internet. They are only accessible from within a virtual network. This is done through private endpoints.

You should use private endpoints to provide more secure communication between your web app and Azure services. By default, service communication to most Azure services crosses the public internet. In the reference implementation, these services include Azure SQL Database, Azure Cache for Redis, and Azure App Service. Azure Private Link enables you to add security to that communication via private endpoints in a virtual network to avoid the public internet.

This improved network security is transparent from the code perspective. It doesn't involve any app configuration, connection string, or code changes.

## Excercise

By default, your user account is added to the application. To enable additional users, follow these steps:

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
