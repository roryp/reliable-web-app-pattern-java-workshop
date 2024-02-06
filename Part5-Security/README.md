# Security

Proseware leverages Microsoft Entra ID to secure using  Managed Identities. 

Microsoft Entra ID uses Roles to assign users and groups to a Azure Entra ID Tenant. The reference implementation creates two app roles (*User* and *Creator*). Roles translate into permissions during authorization. The *Creator* role has permissions to configure the application settings, upload videos, and create playlists. The *User* role can view the videos.

## Exercise - View your *User* 

By default, your user account is added to the application with the "User" role.
In this exercise, we will explore how to view or add additional users.

- Sign in to the [Azure Portal](https://portal.azure.com).
- Select **Microsoft Entra ID** -> **Enterprise Applications**.
- Search for, then select **Proseware**.
- View your Entra ID user account, that is mapped to the application *User* role. Note: This screen also gives access to add future Entra ID users for access to this application

![Proseware's Azure Entra ID Directory enterprise applications](./images/AAD-Enterprise-Application.png)

## Next Up

Next, we will delve into operational excellence in cloud applications --> [Part 6 - Operational Excellence](../Part6-Operational-Excellence/README.md) 

## Resources
[Well-Architected Framework security portal](https://learn.microsoft.com/en-us/azure/well-architected/security)

[Security Checklist](https://learn.microsoft.com/azure/well-architected/security/checklist)
