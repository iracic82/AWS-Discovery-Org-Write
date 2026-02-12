# AWS_Discovery – Infoblox Discovery Role Deployment

This repository provides CloudFormation templates to deploy the **`infoblox_discovery`** IAM role into AWS accounts for **Cloud Assets Insights**.

---

## 🚀 Org-Wide Deployment (Recommended)

Use this option to deploy the role across **all or selected accounts** in your AWS Organization using a CloudFormation **StackSet**.  
You can choose to target:
- Entire **Organizational Units (OUs)**  
- Specific **Account IDs**  
- Or a mix of both (with optional excludes)

The StackSet can also be configured to automatically deploy the role to **new accounts** added to your Org.

### Steps
1. Log in to your **Management Account** (or **CloudFormation delegated admin** account).
2. Make sure **CloudFormation StackSets trusted access** is enabled in AWS Organizations.  
   *(AWS Console → CloudFormation → StackSets → Enable trusted access)*
3. Click the button below.
4. In the Quick Create form, enter one or more of:
   - **TargetOUsCsv** – e.g. `ou-abcd-12345678,ou-efgh-a1b2c3d4`
   - **TargetAccountsCsv** – e.g. `111111111111,222222222222`
   - (Optional) **ExcludeAccountsCsv** – skip accounts when targeting OUs  
5. Choose **Regions** (comma-separated).
6. Click **Create stack**.

[![Deploy Org-Wide][deploy-org-badge]][deploy-org-link]

---

## 🛠 Parameters

- **RoleTemplateURL** – URL of the role definition template (pre-filled).
- **ExternalId** – External ID for third-party access (default provided).
- **AccountId** – Infoblox AWS account ID (default provided).
- **TargetOUsCsv** – Comma-separated **OU IDs** where the role should be deployed.  
  Example: `ou-abcd-12345678,ou-efgh-a1b2c3d4`
- **TargetAccountsCsv** – Comma-separated **AWS Account IDs** to target.  
  Example: `111111111111,222222222222`
- **ExcludeAccountsCsv** – Comma-separated **Account IDs** to exclude when targeting by OU.  
  Example: `333333333333,444444444444`
- **Regions** – AWS region(s) for role deployment.  
  Example: `us-east-1,eu-west-1`
- **AutoDeployNewAccounts** – If `true`, new accounts in the OUs will automatically get the role.

---

## 📌 Notes
- The StackSet creates the IAM role **`infoblox_discovery`** with:
  - `ReadOnlyAccess`
  - `AWSOrganizationsReadOnlyAccess`
- Requires **CAPABILITY_NAMED_IAM** (the Quick Create page pre-ticks this).
- If the role already exists in an account, the StackSet will show an **AlreadyExists** error for that account.
- Quick Create **does not list accounts** — you must paste OU IDs or account IDs manually.  
  Use the AWS CLI to quickly list accounts if needed:
  ```bash
  aws organizations list-accounts \
    --query 'Accounts[?Status==`ACTIVE`].Id' --output text | tr ' ' ','

---

## 🔍 Single-Account Deployment (Optional)

If you only want to deploy the role into **one account**, use this button:

[![Deploy Single Account][deploy-single-badge]][deploy-single-link]

---

## 🖼 AWS Logo

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" alt="AWS Logo" width="100"/>

---

<!-- Reference-style links to keep README clean -->

[deploy-org-badge]: https://img.shields.io/badge/Deploy%20Org--Wide%20(StackSet)-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white
[deploy-org-link]: https://console.aws.amazon.com/cloudformation/home#/stacks/quickcreate?templateURL=https%3A%2F%2Finfoblox-igor.s3.eu-west-1.amazonaws.com%2Finfoblox_discovery_stackset_bootstrap.yaml&stackName=Infoblox-Discovery-Role-StackSet-Bootstrap&param_RoleTemplateURL=https%3A%2F%2Finfoblox-igor.s3.eu-west-1.amazonaws.com%2Finfoblox_discovery_stackset.yaml&param_ExternalId=fd73371c-05e7-4224-bd9f-c072191f66c1&param_AccountId=902917483333&param_TargetOUs=%3CREPLACE_WITH_YOUR_OU_IDS%3E&param_Regions=%3CREPLACE_WITH_YOUR_REGIONS%3E&param_AutoDeployNewAccounts=true

[deploy-single-badge]: https://img.shields.io/badge/Deploy%20Single%20Account-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white
[deploy-single-link]: https://console.aws.amazon.com/cloudformation/home#/stacks/create/template?templateURL=https://infoblox-igor.s3.eu-west-1.amazonaws.com/infoblox_discovery_stackset.yaml
