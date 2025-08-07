# Project-lab25-api-iam-cloudtrail
Secure an AWS API Gateway endpoint using IAM authorization and monitor access with CloudTrail. Includes IAM roles, SigV4 auth, and full audit trail for serverless APIs.
# Lab 25: Advanced API Security with IAM Authorization and CloudTrail Monitoring

## 👤 Author
**Dr. Ime Ben**  
AWS Account ID: `2853-0598-8025`  
GitHub: [ime-cloud-sec-analyst](https://github.com/ime-cloud-sec-analyst)

---

## 🔐 Overview

This lab demonstrates how to securely expose an AWS API Gateway endpoint using **IAM-based authorization** while logging and monitoring all access with **AWS CloudTrail**. The solution implements **enterprise-grade API security**, combining **identity-based access control**, **serverless backend**, and **auditing** — all essential components of a compliant and secure cloud environment.

---

## 🎯 Aim

To restrict public access to an API Gateway endpoint by requiring IAM credentials and monitor all API activities with CloudTrail for security auditing and compliance.

---

## 🧩 Objectives

- 🔐 Restrict access to an API Gateway endpoint using IAM authentication.
- 📜 Enable full API audit logging via CloudTrail.
- 🧪 Simulate real-world authorized and unauthorized API calls.
- ✅ Prove security enforcement in a serverless cloud architecture.

---

## 🛠️ Tools & Services Used

| Tool/Service       | Purpose                                           |
|--------------------|---------------------------------------------------|
| **AWS API Gateway**| Host secure serverless API endpoints              |
| **AWS Lambda**     | Backend function handler                          |
| **AWS IAM**        | Define policies and roles for API authorization   |
| **AWS CloudTrail** | Monitor, log, and audit API usage                 |
| **AWS CLI/Postman**| Test IAM-authenticated API calls                  |
| **S3**             | Store CloudTrail logs securely                    |

---

## ⚙️ Implementation Procedure

### 1. ✅ Prepare Backend Lambda
- Reuse the existing Lambda from Lab 24 (`SecureBackendFunction`).
- Ensure correct execution role and logging permissions.

### 2. 🔗 Configure API Gateway Authorization
- API Gateway → **Routes** → `/SecureBackendFunction`
- Under `Authorization`, select `AWS IAM`
- Save configuration

### 3. 👤 Create IAM User for API Access
- IAM → Add User → `APIAuthorizedUser`
- Enable **programmatic access**
- Attach the following **minimal policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "execute-api:Invoke",
      "Resource": "arn:aws:execute-api:*:*:*"
    }
  ]
}
4. 📡 Simulate API Access with IAM (SigV4)
Option A: AWS CLI

bash
Copy
Edit
aws --region eu-west-1 \
--profile api-user \
apigatewayv2 invoke-api \
--api-id YOUR_API_ID \
--stage dev \
--route-key GET /SecureBackendFunction
Option B: Postman (manual SigV4 headers)

Requires manual setup or AWS Signature tool (e.g., postman-aws-auth).

5. 🕵️ Enable CloudTrail
Go to CloudTrail → Create Trail

Enable management events

Enable data events for API Gateway

Specify a secure S3 bucket for log storage

6. 🔍 Validate Logs
Test access to the endpoint using IAM credentials

Go to CloudTrail > Event History

Filter by:

Event Source = apigateway.amazonaws.com

User Name = APIAuthorizedUser

Review logs for:

IP address

Timestamp

Route accessed

Access success/failure

User identity

🧠 Methods & Security Approaches
Area	Strategy
🔒 Access Control	Enforced via IAM roles and SigV4 authentication
📜 Auditing	Real-time API activity tracking via CloudTrail
🛡️ Principle of Least Privilege	IAM policy scoped to only required execute-api:Invoke action
🧪 Real-world Simulation	IAM user simulates partner/client integration securely
📥 Encrypted Log Storage	CloudTrail logs stored in S3 with server-side encryption

🧩 Use Case & Real-World Importance
IAM-based API access with CloudTrail is critical in the following environments:

🏛️ Regulated industries (Finance, Healthcare, Gov): Compliance with CIS, ISO, PCI-DSS, and NIST requires access control and auditing.

🏢 Enterprise Internal APIs: Secure microservices, partner APIs, or automation tools with controlled IAM access.

🔍 Security Audits & Threat Detection: CloudTrail enables full traceability of API calls and user behavior.

✅ Must-Do Security Practices
Always require IAM or Cognito for sensitive APIs

Enable CloudTrail logs for all regions and services

Use fine-grained IAM policies

Periodically rotate access keys

Block unused or expired IAM users

Automate CloudTrail log analysis with CloudWatch Logs Insights or GuardDuty

⚠️ Challenges Faced
Challenge	Resolution
🔐 Postman lacks native SigV4	Used AWS CLI or AWS Signature Helper
❌ IAM policy misconfiguration	Validated JSON, ensured Resource and Action fields matched
⏱️ CloudTrail log delay	Allowed up to 5 mins before checking event history
📜 Long logs	Used filters and tags to navigate logs quickly

⚠️ Limitations
❌ No route-specific permissions (needs resource policy)

❌ No Cognito user pools or WAF integration yet

❌ Not suitable for public or anonymous APIs

🧪 Limited to AWS IAM; external OAuth not supported in this lab

🚀 Next Steps
🔐 Add Amazon Cognito or OAuth 2.0 for token-based access

🧱 Integrate WAF for SQLi/XSS protection

📈 Enable usage plans and throttling via API Keys

🧪 Create CloudWatch Alarms for high-risk access patterns

📊 Extend audit trails with AWS Config + Security Hub

📁 GitHub Repository
🔗 Repo: lab25-api-iam-cloudtrail

Browse all screenshots, Terraform (if applicable), CloudTrail sample logs, and implementation scripts.

🧾 Summary
Lab 25 closes the AWS Cloud Security Engineer lab portfolio with a critical enterprise-grade security capability: secure API authorization + full observability.

By combining IAM-based access, SigV4 authentication, and CloudTrail monitoring, you implement the gold standard for internal/external API security. This is a must-have pattern in any production AWS environment.
