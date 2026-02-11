# Lab Solution
IAM roles are better than access keys because they allow AWS services like EC2 to access other AWS services without storing credentials on the server. When an IAM role is attached to an EC2 instance, AWS automatically provides temporary credentials through the instance metadata service. This is more secure than using access keys, which can be leaked, hard-coded, or accidentally committed to source control. IAM roles also rotate credentials automatically and follow the principle of least privilege.

1. Created an IAM role with EC2 as the trusted service.
2. Attached the required IAM policies to the role to allow access to AWS services (e.g. S3 and CloudWatch).
3. Attached the IAM role to the EC2 instance.
4. SSH’d into the EC2 instance.
5. Ran AWS CLI commands from inside the instance without running aws configure.
6. Verified access by successfully interacting with AWS services.

The IAM role was given only the permissions required for the lab tasks. S3 permissions to list and upload objects to a specific bucket, allowing the instance to store files in S3. CloudWatch permissions to create log groups and send log data, allowing the instance to publish logs for monitoring. No additional permissions were granted beyond what was necessary, following the principle of least privilege.

Security best practices followed:
- No AWS access keys were created or stored on the EC2 instance.
- Authentication was handled using an IAM role with temporary credentials.
- Least privilege was applied by only granting required permissions.
- Access was tested from within the EC2 instance to ensure permissions worked as intended.
- IAM roles were used instead of long-term credentials to reduce security risk.

Troubleshooting steps:
- Verified that the IAM role was correctly attached to the EC2 instance.
- Confirmed that AWS CLI commands were being run from the instance, not locally.
- Checked IAM policies for missing permissions when commands returned errors.
- Confirmed that no CloudWatch log groups existed until logs or log groups were explicitly created.
- Re-ran AWS CLI commands to confirm successful access after configuration changes.