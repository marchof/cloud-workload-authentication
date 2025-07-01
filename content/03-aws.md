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

## Workloads Identity and Permissions

In AWS, permissions are assigned to workloads through IAM roles. Each workload (such as an EC2 instance, ECS task, Lambda function, or EKS pod) can be configured to assume a specific IAM role. The IAM role acts as the identity of the workload and determines its permissions.

- **Role Assignment:** When launching a compute resource, you specify which IAM role it should assume. For example, an EC2 instance profile or a task role for ECS.
- **Policy Attachment:** Permissions are not granted directly to the workload, but to the IAM role. IAM policies (managed or custom) are attached to the role, defining what AWS API actions are allowed on which resources.
- **Effective Permissions:** When the workload uses its temporary credentials (obtained from the Instance Metadata Service, environment variables, or other supported mechanisms) to access AWS APIs, the platform checks the policies attached to the assumed role and enforces the corresponding permissions.

This approach enables fine-grained, least-privilege access control and makes it easy to manage and audit permissions for workloads at scale.

## Authentication and Authorisation for Platform APIs

Requests to AWS platform APIs are authenticated using the AWS Signature Version 4 (SigV4) signing process. This mechanism ensures that API requests are securely authenticated and authorized using the credentials available to the workload.

- **Authentication:** When a workload needs to call an AWS API, it uses its credentials (access key, secret key, and session token) to sign the request. The signing process (SigV4) creates a cryptographic signature based on the request details and the secret key. This signature is included in the HTTP headers of the API request.
- **Authorization:** When AWS receives the request, it validates the signature and checks the session token. If valid, AWS determines the identity (IAM role) making the request and evaluates the attached IAM policies to authorize the requested action.
- **Technical Implementation:**
    - The AWS SDKs and CLI handle the SigV4 signing process automatically.
    - The signed request includes the `Authorization` header, `x-amz-date`, and (if using temporary credentials) the `x-amz-security-token` header.
    - Example: The application uses the SDK to sign and send API requests, and AWS CloudTrail can be used to audit all actions.

This approach ensures that only workloads with valid, authorized credentials can interact with AWS platform APIs, and all actions are auditable via AWS CloudTrail.

## Local Development

When developing locally, workloads do not have access to AWS-managed IAM roles or the Instance Metadata Service. Instead, developers should use one of the following best practices to authorize local workloads:

- **AWS CLI Configuration:** Use the AWS CLI (`aws configure`) to set up access keys and secret keys in the `~/.aws/credentials` file. The AWS SDKs and CLI will automatically use these credentials for API requests.
- **Environment Variables:** Set the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and (if needed) `AWS_SESSION_TOKEN` environment variables in your development environment. This is useful for temporary credentials, such as those obtained via AWS SSO or STS.
- **AWS SSO (IAM Identity Center):** Use AWS IAM Identity Center (formerly AWS SSO) to obtain temporary credentials for development. Run `aws sso login` to authenticate, and the SDKs will use the SSO profile for API requests.
- **Assume Role:** Use the AWS CLI or SDKs to assume a role with limited permissions for development, rather than using long-lived credentials. This can be done with `aws sts assume-role` and exporting the resulting credentials as environment variables.
- **.env Files and Secret Management:** Never hard-code credentials in source code. Use environment variable files (`.env`) or secret management tools to inject credentials securely during development.

Always follow the principle of least privilege and avoid using production credentials for local development. Regularly rotate credentials and audit their usage to minimize risk.