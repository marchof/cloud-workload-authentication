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
- **Effective Permissions:** When the workload uses its service account credentials to access GCP APIs, the platform checks the roles assigned to that service account and enforces the corresponding permissions.

This approach allows for fine-grained, least-privilege access control and makes it easy to audit and manage permissions for workloads at scale.

## Authentication and Authorisation for Platform APIs

## Local Development