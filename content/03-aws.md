# AWS

## Introduction

Here’s a list of the most important AWS services capable of executing workloads, each with a link to its official documentation:

| Service                                               | Execution Type                   |
| ----------------------------------------------------- | -------------------------------- |
| [EC2](https://docs.aws.amazon.com/ec2/)               | Virtual servers                  |
| [ECS](https://docs.aws.amazon.com/ecs/)               | Managed container orchestration  |
| [EKS](https://docs.aws.amazon.com/eks/)               | Managed Kubernetes clusters      |
| [App Runner](https://docs.aws.amazon.com/apprunner/)  | Serverless containers            |
| [Lambda](https://docs.aws.amazon.com/lambda/)         | Event-driven functions (FaaS)    |



## Security Model

AWS uses a centralized identity and access management system called AWS Identity and Access Management (IAM). Workloads running on AWS services obtain authentication and authorization information through IAM roles, which are securely attached to the compute resource (such as an EC2 instance, ECS task, or Lambda function).

- **Authentication:** When a workload starts, AWS injects temporary security credentials (access key, secret key, and session token) into the environment, typically via the Instance Metadata Service (IMDS) or environment variables. The workload uses these credentials to sign API requests to AWS services.
- **Authorization:** Access to AWS resources is controlled using IAM policies attached to the role. These policies define what actions the workload can perform on which resources. When the workload presents its credentials to an AWS API, the platform checks the permissions and enforces the assigned policies.

This model allows workloads to securely access AWS services and APIs without managing long-term secrets or credentials directly.

## Workloads Identity

## Authentication and Authorisation for Platform APIs

## Local Development