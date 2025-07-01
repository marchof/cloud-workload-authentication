# Fundamentals 

## Key Concepts and Terms

### Workload
Any application, service, container, function, or process that performs tasks in a cloud or distributed system. Workloads are the entities that require authentication and authorization to access resources and APIs.

### Identity
A unique representation of a workload, often managed by the platform (e.g., service account, managed identity, IAM role). The identity is used to distinguish one workload from another and is the basis for assigning permissions and tracking actions.

### Authentication
The process of verifying the identity of a workload. This is typically achieved using credentials such as tokens, certificates, or keys. Authentication ensures that only legitimate workloads can access protected resources.

### Authorization
The process of determining what actions an authenticated workload is allowed to perform. Authorization is usually enforced by policies or roles, and ensures that workloads can only access resources and perform actions they are permitted to.

### Credential
A secret or token used to prove a workload's identity (e.g., OAuth token, X.509 certificate, API key). Credentials must be protected to prevent unauthorized access and impersonation.

### Federation
A trust relationship between different identity providers or platforms, allowing workloads to access resources across boundaries. Federation enables single sign-on and cross-platform access without duplicating identities.

### RBAC (Role-Based Access Control)
An authorization model where permissions are assigned to roles, and roles are assigned to identities. RBAC simplifies permission management and supports the principle of least privilege.

### Principle of Least Privilege
The practice of granting workloads only the permissions they need to perform their tasks, and no more. This reduces the risk of accidental or malicious misuse of privileges.

### Zero Trust
A security model that assumes no implicit trust, requiring continuous verification of identity and permissions for every request. Zero Trust minimizes the risk of lateral movement and privilege escalation within a system.


## Standards and Protocols

Below is a list of key standards and protocols referenced throughout this repository, with short descriptions and links to their official definitions:

- **OpenID Connect (OIDC):** An identity layer on top of OAuth 2.0 for federated authentication, widely used for workload identity federation. [OIDC Standard](https://openid.net/connect/)
- **OAuth 2.0:** An authorization framework that enables applications to obtain limited access to user accounts on an HTTP service. [OAuth 2.0 RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)
- **SAML 2.0:** Security Assertion Markup Language, a standard for exchanging authentication and authorization data between parties, commonly used for enterprise federation. [SAML 2.0 Standard](https://docs.oasis-open.org/security/saml/v2.0/)
- **X.509 Certificates:** A standard defining the format of public key certificates, used for mutual TLS and other authentication scenarios. [X.509 Standard](https://www.itu.int/rec/T-REC-X.509/en)
- **Mutual TLS (mTLS):** A protocol where both client and server authenticate each other using X.509 certificates. [mTLS Overview](https://datatracker.ietf.org/doc/html/rfc8705)
- **JSON Web Token (JWT):** A compact, URL-safe means of representing claims to be transferred between two parties, used in OIDC, OAuth, and cloud identity federation. [JWT RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519)
- **Role-Based Access Control (RBAC):** An approach to restricting system access to authorized users based on roles. [NIST RBAC Standard](https://csrc.nist.gov/projects/role-based-access-control)
- **Security Token Service (STS):** A protocol/service for issuing, validating, and exchanging security tokens (e.g., AWS STS, Azure AD, GCP IAM). [WS-Trust STS](https://docs.oasis-open.org/ws-sx/ws-trust/v1.4/ws-trust.html)

## Common Authentication Mechanisms

- **Service Accounts:** Platform-managed identities for workloads (e.g., Kubernetes service accounts, GCP service accounts).
- **Managed Identities:** Cloud provider-managed identities for workloads (e.g., Azure Managed Identity, AWS IAM roles).
- **Short-Lived Credentials:** Temporary tokens or certificates that reduce the risk of credential leakage.
- **Mutual TLS (mTLS):** Both client and server authenticate each other using certificates.
- **OIDC/OAuth 2.0:** Open standards for federated authentication and authorization.

## Best Practices

- Use platform-managed identities whenever possible.
- Avoid hard-coding credentials in source code or configuration files.
- Regularly rotate credentials and use short-lived tokens.
- Apply least privilege and zero trust principles.
- Monitor and audit workload authentication and authorization events.

For more details, see the platform-specific chapters in this guide.
