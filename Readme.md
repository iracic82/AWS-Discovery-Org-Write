# AWS_Discovery_RW – Infoblox Discovery & DNS Automation Role

This repository provides CloudFormation templates to deploy the **`infoblox_discovery`**
IAM role into AWS accounts for Infoblox Cloud Assets Insights and DNS automation.

This version includes:

- AWS Organizations visibility
- EC2 discovery permissions
- Route53 read access
- Route53 write capabilities (hosted zone & record management)

---

## 🔐 Security Model

The role:

- Uses cross-account trust with **ExternalId enforcement**
- Restricts assumption to a specific AWS account
- Is deployed consistently via CloudFormation StackSet
- Should NOT be manually modified in target accounts

---

## 🚀 Org-Wide Deployment (Recommended)

Deploy across your AWS Organization using a CloudFormation **StackSet**.

### Steps

1. Log in to your **AWS Organizations Management Account** (or delegated admin).
2. Ensure **CloudFormation StackSets trusted access** is enabled.
3. Click the button below.
4. Enter your target OU IDs.
5. Choose **one region** (IAM is global).
6. Create the stack.

[![Deploy Org-Wide][deploy-org-badge]][deploy-org-link]

---

## 🛠 Parameters

| Parameter | Description |
|------------|-------------|
| `RoleTemplateURL` | HTTPS S3 URL of the IAM role template |
| `ExternalId` | External ID required for Infoblox |
| `AccountId` | Infoblox AWS account ID allowed to assume the role |
| `TargetOUsCsv` | Comma-separated OU IDs |
| `Regions` | One region only (IAM is global) |
| `AutoDeployNewAccounts` | Deploy role automatically to new accounts |

---

## 🔍 Permissions Included

### Managed Policy
- `AWSOrganizationsReadOnlyAccess`

### Inline Policy
- EC2 discovery (`Describe*`)
- Route53 list and get
- Route53 record changes
- Hosted zone creation & deletion
- Hosted zone updates

⚠ This role includes DNS write permissions. Deploy only if automation is required.

---

## 📌 Important Notes

- Requires `CAPABILITY_NAMED_IAM`
- IAM is global per account — select only one region
- Do not manually edit role in member accounts
- Update permissions via StackSet only

---

## 🏗 Architecture Overview

Management Account  
→ CloudFormation StackSet  
→ Target OUs  
→ IAM Role `infoblox_discovery`  

Cross-account assumption secured by:
- ExternalId
- Explicit Principal restriction

---

<!-- Update this link after uploading templates -->

[deploy-org-badge]: https://img.shields.io/badge/Deploy%20Org--Wide%20(StackSet)-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white
[deploy-org-link]: https://console.aws.amazon.com/cloudformation/home#/stacks/quickcreate?templateURL=https%3A%2F%2Finfoblox-igor.s3.eu-west-1.amazonaws.com%2Finfoblox_discovery_stackset_bootstrap_rw.yaml&stackName=Infoblox-Discovery-Role-StackSet-Bootstrap&param_RoleTemplateURL=https%3A%2F%2Finfoblox-igor.s3.eu-west-1.amazonaws.com%2Finfoblox_discovery_role_rw.yaml&param_ExternalId=fd73371c-05e7-4224-bd9f-c072191f66c1&param_AccountId=902917483333&param_TargetOUsCsv=%3CREPLACE_WITH_YOUR_OU_IDS%3E&param_Regions=eu-west-1&param_AutoDeployNewAccounts=true
