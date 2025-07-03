# 4. Azure

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

Requests to Azure platform APIs are authenticated using OAuth 2.0 access tokens issued by Azure Active Directory (Azure AD, now Microsoft Entra ID). This ensures that API requests are securely authenticated and authorized using the identity assigned to the workload.

- **Authentication:** When a workload needs to call an Azure API, it uses its Managed Identity to request an OAuth 2.0 access token from Azure AD for the target resource (API). The token is included in the `Authorization` header as a Bearer token in the API request.
- **Authorization:** When Azure receives the request, it validates the access token and determines the identity (Managed Identity) making the request. Azure then evaluates the RBAC roles assigned to that identity to authorize the requested action.
- **Technical Implementation:**
    - The Azure SDKs and CLI handle token acquisition and request signing automatically.
    - The access token is included in the `Authorization: Bearer <token>` header.
    - Example: The application uses the Azure SDK to obtain a token via the Instance Metadata Service endpoint (`http://169.254.169.254/metadata/identity/oauth2/token`) and then calls the Azure API with the token.

This approach ensures that only workloads with valid, authorized identities can interact with Azure platform APIs, and all actions are auditable via Azure Activity Log.

## Local Development

When developing locally, workloads do not have access to Azure-managed identities or the Instance Metadata Service. Instead, developers should use one of the following best practices to authorize local workloads:

- **Azure CLI Authentication:** Use the Azure CLI (`az login`) to authenticate with your Azure account. The Azure SDKs and CLI will automatically use this authentication context for API requests.
- **Environment Variables:** Set the `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, and `AZURE_CLIENT_SECRET` environment variables to use a service principal for local development. This is useful for automation and CI/CD scenarios.
- **Managed Identity Emulation:** Use tools like Azure Identity SDK's `DefaultAzureCredential`, which will try multiple authentication methods (including Azure CLI, environment variables, and managed identity if available).
- **.env Files and Secret Management:** Never hard-code credentials in source code. Use environment variable files (`.env`) or secret management tools to inject credentials securely during development.

Always follow the principle of least privilege and avoid using production credentials for local development. Regularly rotate credentials and audit their usage to minimize risk.

## Federation with External Identity Providers

Azure supports federation with external identity providers using Azure AD Workload Identity Federation. You can configure Azure AD to trust external OIDC providers and map external identities to Azure AD app registrations or service principals. Role assignments are managed using Azure RBAC.

- **Tools:**
    - Azure AD App Registrations
    - Federated Credentials (OIDC)
    - Azure RBAC
- **Configuration:**
    - Register an app in Azure AD and configure federated credentials
    - Map external claims (e.g., `sub`, `aud`, `iss`) to Azure AD identities
    - Assign roles to the app/service principal using Azure RBAC
- **References:**
    - https://learn.microsoft.com/azure/active-directory/develop/workload-identity-federation
