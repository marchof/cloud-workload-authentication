# Azure

## Introduction

Here’s a list of the most important Azure services capable of executing workloads, each with a link to its official documentation:

| Service                                                      | Execution Type                   |
| ------------------------------------------------------------ | -------------------------------- |
| [Virtual Machines](https://learn.microsoft.com/azure/virtual-machines/) | Virtual servers                  |
| [Azure Container Instances](https://learn.microsoft.com/azure/container-instances/) | Managed containers               |
| [Azure Kubernetes Service (AKS)](https://learn.microsoft.com/azure/aks/) | Managed Kubernetes clusters      |
| [App Service](https://learn.microsoft.com/azure/app-service/) | Managed web apps & APIs          |
| [Functions](https://learn.microsoft.com/azure/azure-functions/) | Event-driven functions (FaaS)    |

## Security Model

Azure uses a centralized identity and access management system based on Azure Active Directory (Azure AD, now called Microsoft Entra ID). Workloads running on Azure services can obtain authentication and authorization information through Managed Identities, which are service principals managed by Azure.

- **Authentication:** Workloads authenticate themselves to Azure platform APIs using Managed Identities. When enabled, Azure injects identity credentials into the environment (such as via the Instance Metadata Service or environment variables). The workload can then request an OAuth 2.0 access token from Azure AD for a specific resource or API.
- **Authorization:** Access to Azure resources is controlled using Azure Role-Based Access Control (RBAC). The Managed Identity is assigned roles that define what actions it can perform on which resources. When the workload presents its access token to an Azure API, the platform checks the token and enforces the assigned permissions.

This model allows workloads to securely access Azure services and APIs without managing secrets or credentials directly.

## Workloads Identity and Permissions

In Azure, permissions are assigned to workloads through Managed Identities. Each workload (such as a Virtual Machine, App Service, Function, or AKS pod) can be configured to use a system-assigned or user-assigned Managed Identity. The Managed Identity acts as the identity of the workload and determines its permissions.

- **Managed Identity Assignment:** When deploying a resource, you enable a Managed Identity for it. For example, you can enable a system-assigned identity for a VM or assign a user-assigned identity to an App Service or AKS pod.
- **Role Assignment (RBAC):** Permissions are not granted directly to the workload, but to the Managed Identity. Azure RBAC roles (such as Reader, Contributor, or custom roles) are assigned to the Managed Identity at the subscription, resource group, or resource level.
- **Effective Permissions:** When the workload uses its Managed Identity to request an access token and access Azure APIs, the platform checks the roles assigned to that identity and enforces the corresponding permissions.

This approach enables fine-grained, least-privilege access control and simplifies management and auditing of permissions for workloads at scale.

## Authentication and Authorisation for Platform APIs

## Local Development
