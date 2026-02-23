# Lambda Function Upload to S3 with IaC

This project demonstrates how to automate Lambda function uploads to S3 using Terraform and GitHub Actions.

## Architecture

- **Terraform**: Provisions S3 bucket with versioning and security controls
- **Lambda Function**: Sample Python function
- **GitHub Actions**: Automates deployment on PR merge

## Prerequisites

- AWS Account with appropriate permissions
- Terraform installed
- GitHub repository
- AWS CLI (optional, for testing)

## Setup Instructions

### 1. Provision Infrastructure with Terraform

```bash
# Initialize Terraform
terraform init

# Review the plan
terraform plan

# Apply the configuration
terraform apply
```

Note the bucket name from the output.

### 2. Configure GitHub Secrets

Add these secrets to your GitHub repository (Settings → Secrets and variables → Actions):

- `AWS_ACCESS_KEY_ID`: Your AWS access key
- `AWS_SECRET_ACCESS_KEY`: Your AWS secret key
- `AWS_REGION`: AWS region (e.g., us-east-1)
- `S3_BUCKET_NAME`: The bucket name from Terraform output

### 3. Customize Configuration (Optional)

Edit `variables.tf` to change:
- `aws_region`: Your preferred AWS region
- `bucket_name`: Your desired bucket name (must be globally unique)

## How It Works

1. **Infrastructure**: Terraform creates an S3 bucket with:
   - Versioning enabled
   - Public access blocked
   - Secure configuration

2. **Deployment**: When a PR is merged to main:
   - GitHub Actions checks out the code
   - Zips the Lambda function
   - Uploads to S3 with timestamp and latest version

## File Structure

```
.
├── main.tf                    # Terraform main configuration
├── variables.tf               # Terraform variables
├── outputs.tf                 # Terraform outputs
├── lambda/
│   └── lambda_function.py     # Sample Lambda function
└── .github/
    └── workflows/
        └── deploy-lambda.yml  # GitHub Actions workflow
```

## Testing

After setup, create a PR with changes to the Lambda function and merge it. Check your S3 bucket for the uploaded zip files.

## Cleanup

To destroy the infrastructure:

```bash
terraform destroy
```

Note: Empty the S3 bucket before destroying, or add a force_destroy flag to the bucket resource.
