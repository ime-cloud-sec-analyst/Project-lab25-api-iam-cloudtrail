# Project-lab25-api-iam-cloudtrail
Secure an AWS API Gateway endpoint using IAM authorization and monitor access with CloudTrail. Includes IAM roles, SigV4 auth, and full audit trail for serverless APIs.
Lab 25: Advanced API Security with IAM Authorization and CloudTrail Monitoring
Author
Dr. Ime Ben
AWS Account ID: 2853-0598-8025
Overview
This lab demonstrates how to implement strong IAM-based authorization for AWS API Gateway endpoints and monitor all activity using AWS CloudTrail. 
We build on top of the previous serverless API setup, enhancing security, access control, and observability.
Aim
To secure a public-facing API Gateway endpoint using AWS IAM authorization and monitor the access via CloudTrail logs for audit and compliance.
Objectives
- Restrict access to an API Gateway endpoint using IAM roles.
- Set up CloudTrail to capture API invocation logs.
- Simulate access with valid IAM user credentials.
- Demonstrate security enforcement in a real-world serverless architecture.
Tools Used
- AWS API Gateway (HTTP API)
- AWS Lambda
- IAM Roles and Policies
- AWS CloudTrail
- AWS CLI or Postman with SigV4 authorization
Implementation Procedure
Step 1: Prepare Lambda Function
Use the same `SecureBackendFunction` created in Lab 24. Confirm the role has execution permissions and logging.
Step 2: Modify API Gateway Integration
Go to API Gateway > SecureBackendAPI
Click on Routes > /SecureBackendFunction
Under Authorization, select AWS IAM
Save changes
Step 3: Create an IAM User for Testing
IAM > Users > Add user
Name: APIAuthorizedUser
Programmatic access: ✅
Attach policy:
{
"Version": "2012-10-17",
"Statement": [{"Effect": "Allow","Action": "execute-api:Invoke","Resource": "arn:aws:execute-api:*:*:*"}]
}
Save access key and secret key.
Step 4: Test IAM Authorization (Postman or AWS CLI)
Use SigV4 Signing (required for IAM authorization)
Example with AWS CLI:
aws apigatewayv2 get-api --api-id YOUR_API_ID
aws --region eu-west-1 --profile api-user apigatewayv2 invoke-api --api-id YOUR_API_ID --stage dev --route-key GET /SecureBackendFunction
Step 5: Enable CloudTrail Monitoring
Go to CloudTrail > Create Trail
Enable management events ✅
Enable data events > Select API Gateway
Specify S3 bucket for logs
Finish setup
Step 6: Validate Logs
Access the API with the IAM user
Visit CloudTrail > Event History
Filter by Event Source: apigateway.amazonaws.com
View IP address, request ID, IAM user, method used, etc.
Strategies and Approach
- Enforce IAM over public APIs to prevent unauthorized access.
- Use CloudTrail for a tamper-proof audit trail.
- Apply least-privilege access through scoped IAM policies.
- Combine SigV4 + IAM roles for secured automation scripts.
Significance in the Real Work Environment
- Security-first development: IAM access is controlled per API
- Auditable: CloudTrail offers full traceability of API activity
- Policy-driven Access: Aligns with compliance frameworks (CIS, ISO)
- Enterprise-ready: IAM + CloudTrail is mandatory in regulated environments
Must-Do Best Practices
- Always use IAM for internal or partner-access APIs
- Regularly rotate access keys
- Log API access with CloudTrail and store in encrypted S3
- Deny open/insecure permissions explicitly
- Validate SigV4 auth in production testing
Challenges Encountered
- Postman lacks native SigV4; required manual header setup or use of CLI
- Misconfigured IAM policy causes 403 even for valid users
- CloudTrail log delivery delay (~5 mins)
Limitations
- No fine-grained access per route (unless using Resource policies)
- No WAF or Cognito integration in this stage
- Not suitable for anonymous/public access use cases
Next Steps
- Integrate Cognito for token-based access
- Add WAF for additional filtering
- Implement throttling and usage plans with API keys
- Extend audit with CloudWatch Alarms on suspicious activity
GitHub Repository
https://github.com/ime-cloud-sec-analyst/lab25-api-iam-cloudtrail
Summary
This final lab project closes the loop in serverless security by showing how to apply IAM-based access control and traceable event logging for APIs. This pattern is essential for securing internal APIs and complying with enterprise security requirements.

By using IAM + CloudTrail, we protect, monitor, and audit every request — a critical skill for any modern AWS Cloud Security Engineer.
