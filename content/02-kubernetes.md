# Kubernetes

## Introduction

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. The most important workload types in Kubernetes are:

| Workload Type         | Description                                 |
|----------------------|---------------------------------------------|
| Pod                  | The smallest deployable unit in Kubernetes. |
| Deployment           | Manages stateless applications.             |
| StatefulSet          | Manages stateful applications.              |
| DaemonSet            | Ensures a copy of a pod runs on each node.  |
| Job/CronJob          | Manages batch and scheduled jobs.           |

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)

## Security Model

Kubernetes uses a layered security model. Authentication and authorization are handled by the API server, which supports multiple authentication methods (certificates, bearer tokens, OIDC, etc.) and authorization modules (RBAC, ABAC, Webhook).

- **Authentication:** Workloads and users authenticate to the API server using service account tokens, certificates, or external identity providers (OIDC).
- **Authorization:** Access to resources is controlled using Role-Based Access Control (RBAC) or other authorization modules. Permissions are granted to users or service accounts via roles and role bindings.

## Workloads Identity and Permissions

In Kubernetes, workloads are typically assigned identities via service accounts. These service accounts are used by pods to authenticate to the API server and, optionally, to external systems.

- **Service Account Assignment:** Each pod runs as a service account, either the default or a custom one specified in the pod spec.
- **RBAC Role Binding:** Permissions are granted to service accounts using RBAC roles and role bindings, defining what actions the workload can perform within the cluster.
- **External Identity Integration:** For cloud-managed clusters (e.g., EKS, GKE, AKS), service accounts can be mapped to cloud IAM identities for access to cloud APIs.

## Authentication and Authorisation for Platform APIs

Requests to the Kubernetes API server are authenticated using service account tokens, client certificates, or external identity providers. Authorization is enforced by RBAC or other configured modules.

- **Authentication:** Pods use service account tokens (automatically mounted in `/var/run/secrets/kubernetes.io/serviceaccount/token`) to authenticate to the API server. External users may use certificates or OIDC tokens.
- **Authorization:** The API server checks the user's or service account's permissions via RBAC or other modules before allowing access to resources.
- **Technical Implementation:**
    - The Kubernetes client libraries and `kubectl` handle token usage and request signing automatically.
    - The token is included in the `Authorization: Bearer <token>` header.
    - Example: A pod uses its service account token to call the API server, and the server enforces RBAC rules.

## Local Development

When developing locally, workloads and users can authenticate to a Kubernetes cluster using kubeconfig files, which may contain client certificates, tokens, or OIDC credentials.

- **Kubeconfig:** Use `kubectl config` to manage kubeconfig files for local authentication.
- **Minikube/KinD:** Local clusters like Minikube or KinD provide their own authentication setup, usually via certificates or tokens.
- **Service Account Emulation:** For testing, you can use service account tokens or impersonate service accounts with `kubectl`.
- **Least Privilege:** Always use the minimum permissions required for development and avoid using cluster-admin roles.

Always secure kubeconfig files and avoid sharing credentials. Regularly review and rotate credentials as needed.

## Federation with External Identity Providers

Kubernetes supports federation with external identity providers using OpenID Connect (OIDC). The API server can be configured with the `--oidc-*` flags to trust an external OIDC provider (such as AWS, Azure AD, GCP, or GitHub). External identities are mapped to Kubernetes users or service accounts, and their permissions are managed using RBAC.

- **Tools:**
    - API server OIDC flags (`--oidc-issuer-url`, `--oidc-client-id`, etc.)
    - RBAC for mapping external identities to roles
- **Configuration:**
    - Configure the API server to trust the external OIDC provider
    - Use RBAC role bindings to assign permissions to external identities (by subject or group claim)
- **References:**
    - https://kubernetes.io/docs/reference/access-authn-authz/authentication/#openid-connect-tokens

## OpenShift: Differences from Plain Kubernetes

OpenShift is an enterprise Kubernetes distribution by Red Hat that extends upstream Kubernetes with additional features, security controls, and opinionated defaults. Here are some key differences relevant to workload authentication and identity:

- **Integrated OAuth Server:** OpenShift includes a built-in OAuth server for user authentication, supporting multiple identity providers (LDAP, GitHub, OIDC, etc.).
- **Service Account Tokens:** Like Kubernetes, OpenShift uses service accounts for workload identity, but adds stricter defaults and additional controls for token management and rotation.
- **RBAC and SCCs:** OpenShift enforces Role-Based Access Control (RBAC) and introduces Security Context Constraints (SCCs) for fine-grained workload security.
- **User and Group Management:** OpenShift provides enhanced user and group management, including mapping external identities to OpenShift users and groups.
- **OAuth Proxy and Service Mesh:** OpenShift Service Mesh and OAuth Proxy can be used to secure workload-to-workload and ingress traffic with integrated authentication.
- **Console and CLI Integration:** The OpenShift web console and `oc` CLI provide integrated authentication and authorization flows, leveraging the platform's OAuth server.

**References:**
- [OpenShift Authentication Overview](https://docs.openshift.com/container-platform/latest/authentication/index.html)
- [Security Context Constraints](https://docs.openshift.com/container-platform/latest/authentication/managing-security-context-constraints.html)
- [OpenShift Service Mesh](https://docs.openshift.com/container-platform/latest/service_mesh/index.html)
