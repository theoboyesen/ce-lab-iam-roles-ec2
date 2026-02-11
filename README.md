# Lab M2.05 - IAM Roles for EC2

**Repository:** [https://github.com/cloud-engineering-bootcamp/ce-lab-iam-roles-ec2](https://github.com/cloud-engineering-bootcamp/ce-lab-iam-roles-ec2)

**Activity Type:** Individual  
**Estimated Time:** 30-45 minutes

## Learning Objectives

- [ ] Create IAM role for EC2 instances
- [ ] Attach policies to IAM roles
- [ ] Assign IAM role to running EC2 instance
- [ ] Test AWS API access using role
- [ ] Verify security without hardcoded credentials

## Your Task

Create and assign an IAM role that allows your EC2 instance to:
1. Read from specific S3 bucket
2. Write logs to CloudWatch
3. No other permissions (least privilege)

**Success:** Access S3 from EC2 without aws configure

## Quick Steps

```bash
# 1. Create IAM role (via console)
# Name: ec2-s3-cloudwatch-role
# Trusted entity: EC2
# Policies:
#   - CloudWatchAgentServerPolicy (managed)
#   - Custom S3 policy (create):
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject", "s3:PutObject", "s3:ListBucket"],
    "Resource": [
      "arn:aws:s3:::custom-s3-policy-ih26/*",
      "arn:aws:s3:::custom-s3-policy-ih26"
    ]
  }]
}

# 2. Attach role to instance
# EC2 Console → Instance → Actions → Security → Modify IAM role

# 3. Test from instance (NO aws configure needed!)
ssh instance
aws s3 ls s3://YOUR-BUCKET-NAME/
echo "test" > test.txt
aws s3 cp test.txt s3://YOUR-BUCKET-NAME/

# 4. Verify using role
aws sts get-caller-identity
# Should show assumed-role
```

## 📤 What to Submit

**Submission Type:** File Upload (ZIP)

Create a ZIP file named `lab-m2-iam-roles.zip` containing:

1. **IAM Policy Files:**
   - `s3-cloudwatch-policy.json` (your custom IAM policy)
   - `trust-policy.json` (EC2 assume role policy)

2. **Test Output:**
   - `aws-test-commands.txt` (commands you ran and their output)
   - `s3-test-output.txt` (S3 upload test results)
   - `cloudwatch-test-output.txt` (CloudWatch log test)

3. **Screenshots** (in `screenshots/` folder):
   - IAM role creation in console
   - Policy attachment
   - EC2 instance with role attached (in console)
   - `aws sts get-caller-identity` showing assumed role
   - Successful S3 file upload
   - CloudWatch logs showing your test data

4. **Documentation** (`README.md`):
   - Why IAM roles are better than access keys
   - Step-by-step process
   - Policy explanation (what permissions you granted and why)
   - Security best practices you followed
   - Any troubleshooting steps

**File Structure:**
```
lab-m2-iam-roles.zip
├── README.md
├── policies/
│   ├── s3-cloudwatch-policy.json
│   └── trust-policy.json
├── test-output/
│   ├── aws-test-commands.txt
│   ├── s3-test-output.txt
│   └── cloudwatch-test-output.txt
└── screenshots/
    ├── 01-role-creation.png
    ├── 02-policy-attachment.png
    ├── 03-ec2-with-role.png
    ├── 04-assumed-role-identity.png
    ├── 05-s3-upload-success.png
    └── 06-cloudwatch-logs.png
```

## Grading: 100 points
- IAM role creation: 25pts
- Policy configuration (least privilege): 30pts
- Testing and verification: 25pts
- Documentation and security analysis: 20pts
