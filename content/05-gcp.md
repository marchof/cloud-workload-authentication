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

## Workloads Identity

## Authentication and Authorisation for Platform APIs

## Local Development