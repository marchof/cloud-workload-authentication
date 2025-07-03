# 7. Interoperability: Cross-Platform Workload Authentication

## Introduction

In multi-cloud and hybrid environments, workloads often need to access resources or APIs across different platforms (e.g., an AWS Lambda function calling an Azure API, or a Kubernetes workload accessing GCP services). Securely authenticating workloads across platform boundaries is essential for enabling interoperability while maintaining strong security guarantees.

## Techniques for Cross-Platform Authentication

### 1. OIDC Federation
OpenID Connect (OIDC) federation allows one platform to trust identities issued by another. For example, AWS, Azure, and GCP all support OIDC-based workload identity federation, enabling a workload running on one platform to obtain credentials for another by presenting an OIDC token.
- **Example:** An AWS workload can use its OIDC identity to assume a role in GCP or Azure, and vice versa.
- **How it works:**
    - The source platform issues an OIDC token for the workload.
    - The target platform is configured to trust the OIDC issuer and map claims to permissions.
    - The workload exchanges the OIDC token for platform-native credentials (e.g., AWS STS, Azure AD, GCP IAM).

### 2. Service Account Impersonation
Some platforms allow workloads to impersonate service accounts or roles in another platform, often using short-lived tokens or signed JWTs.
- **Example:** A GCP workload can use workload identity federation to impersonate an AWS IAM role.

### 3. External Identity Providers
Cloud platforms can be configured to trust external identity providers (IdPs), such as Active Directory, Okta, or custom OIDC providers. This enables workloads authenticated by the IdP to access resources across multiple clouds.

### 4. API Keys and Custom Tokens
As a fallback, workloads can use API keys or custom tokens to authenticate across platforms. However, this approach is less secure and should be avoided in favor of federated or managed identity solutions.

## Best Practices
- Prefer OIDC federation or managed identity federation over static credentials or API keys.
- Use short-lived, auditable credentials for cross-platform access.
- Limit the scope and permissions of federated identities to the minimum required.
- Monitor and audit cross-platform authentication events.

For platform-specific setup and examples, see the respective chapters in this guide.

## Cross-Platform Authentication & Authorization Matrix

The following matrix summarizes recommended techniques for authenticating and authorizing workloads from each platform to access resources on the others (sources are rows, targets are columns):

| Source \ Target   | Kubernetes | AWS | Azure | GCP | GitHub Actions |
|-------------------|------------|-----|-------|-----|----------------|
| **Kubernetes**    | Native RBAC, Service Accounts | OIDC Federation (EKS, IRSA) | OIDC Federation (AKS, Azure AD Workload Identity) | OIDC Federation (GKE Workload Identity Federation) | OIDC Token, GitHub OIDC Trust |
| **AWS**           | OIDC Token, Kubernetes API | Native IAM | OIDC Federation (Azure AD trusts AWS OIDC) | OIDC Federation (GCP trusts AWS OIDC) | OIDC Token, GitHub OIDC Trust |
| **Azure**         | OIDC Token, Kubernetes API | OIDC Federation (AWS trusts Azure AD) | Native Managed Identity | OIDC Federation (GCP trusts Azure AD) | OIDC Token, GitHub OIDC Trust |
| **GCP**           | OIDC Token, Kubernetes API | OIDC Federation (AWS trusts GCP) | OIDC Federation (Azure AD trusts GCP) | Native IAM | OIDC Token, GitHub OIDC Trust |
| **GitHub Actions**| OIDC Token, Kubernetes API | OIDC Federation (AWS OIDC Trust for GitHub Actions) | OIDC Federation (Azure AD OIDC Trust for GitHub Actions) | OIDC Federation (GCP OIDC Trust for GitHub Actions) | Native GitHub Token |

**Legend:**
- **Native**: Uses the platform's built-in identity and authorization system.
- **OIDC Federation**: Uses OpenID Connect federation to trust external workload identities.
- **OIDC Token**: Uses an OIDC token issued by the source platform for authentication.

> For setup details and examples, see the platform-specific chapters in this guide.
