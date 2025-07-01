# Google Cloud Platform (GCP)

## Introduction

Here’s a list of the most important GCP services capable of executing workloads, each with a link to its official documentation:

| Service                                                 | Execution Type                 |
| ------------------------------------------------------- | ------------------------------ |
| [Compute Engine](https://cloud.google.com/compute/)     | Virtual servers                |
| [App Engine](https://cloud.google.com/appengine/)       | PaaS for web apps              |
| [GKE](https://cloud.google.com/kubernetes-engine/)      | Container orchestration (K8s)  |
| [Cloud Run](https://cloud.google.com/run/)              | Serverless containers          |
| [Cloud Functions](https://cloud.google.com/functions/)  | Event-driven functions (FaaS)  |


## Security Model

GCP uses a unified identity and access management system called Google Cloud IAM. Workloads running on GCP services obtain authentication and authorization information through service accounts, which are identities managed by Google Cloud.

- **Authentication:** When a workload starts, GCP injects service account credentials into the environment, typically via the metadata server or environment variables. The workload uses these credentials to obtain OAuth 2.0 access tokens or JWTs, which are used to authenticate to GCP APIs.
- **Authorization:** Access to GCP resources is controlled using IAM roles and policies attached to the service account. These roles define what actions the workload can perform on which resources. When the workload presents its token to a GCP API, the platform checks the token and enforces the assigned permissions.

This model allows workloads to securely access GCP services and APIs without managing long-term secrets or credentials directly.

## Workloads Identity and Permissions

In GCP, permissions are assigned to workloads through service accounts. Each workload (such as a VM, container, or function) can be configured to run as a specific service account. IAM roles are then granted to that service account, defining what actions it can perform on which resources.

- **Service Account Assignment:** When deploying a workload, you specify which service account it should use. For example, a Compute Engine VM or a Cloud Run service can be configured to run as a particular service account.
- **IAM Role Binding:** Permissions are not granted directly to the workload, but to the service account identity. IAM roles (such as Viewer, Editor, or custom roles) are bound to the service account at the project, folder, or resource level.
- **Effective Permissions:** When the workload uses its service account credentials (obtained from the metadata server, environment variables, or other supported mechanisms) to access GCP APIs, the platform checks the roles assigned to that service account and enforces the corresponding permissions.

This approach allows for fine-grained, least-privilege access control and makes it easy to audit and manage permissions for workloads at scale.

## Authentication and Authorisation for Platform APIs

Requests to GCP platform APIs are authenticated using OAuth 2.0 access tokens or JWTs issued by Google Cloud IAM. This ensures that API requests are securely authenticated and authorized using the service account identity assigned to the workload.

- **Authentication:** When a workload needs to call a GCP API, it uses its service account credentials to obtain an OAuth 2.0 access token or sign a JWT for the target API. The token or JWT is included in the `Authorization` header as a Bearer token in the API request.
- **Authorization:** When GCP receives the request, it validates the token or JWT and determines the identity (service account) making the request. GCP then evaluates the IAM roles assigned to that identity to authorize the requested action.
- **Technical Implementation:**
    - The Google Cloud SDKs and client libraries handle token acquisition and request signing automatically.
    - The access token or JWT is included in the `Authorization: Bearer <token>` header.
    - Example: The application uses the metadata server endpoint (`http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token`) to obtain a token and then calls the GCP API with the token.

This approach ensures that only workloads with valid, authorized identities can interact with GCP platform APIs, and all actions are auditable via Cloud Audit Logs.

## Local Development

When developing locally, workloads do not have access to GCP-managed service accounts or the metadata server. Instead, developers should use one of the following best practices to authorize local workloads:

- **gcloud CLI Authentication:** Use the Google Cloud CLI (`gcloud auth application-default login`) to authenticate with your Google account. The Google Cloud SDKs and client libraries will automatically use these credentials for API requests.
- **Service Account Keys:** Download a service account key in JSON format from the Google Cloud Console and set the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to point to this file. This allows the SDKs to use the service account for authentication.
- **Environment Variables and Secret Management:** Never hard-code credentials in source code. Use environment variable files (`.env`) or secret management tools to inject credentials securely during development.
- **IAM Roles:** Use service accounts with least-privilege IAM roles for development, and avoid using production credentials.

Always follow the principle of least privilege and avoid using production credentials for local development. Regularly rotate credentials and audit their usage to minimize risk.