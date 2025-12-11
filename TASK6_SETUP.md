# Task 6: GitHub Actions CI/CD Setup - READY TO PUSH ✅

## Validation Complete

All components have been tested and validated:

✅ **Docker Build**: Successfully built `Dockerfile.ci`  
✅ **Terraform Format**: Passed  
✅ **Terraform Validate**: Passed  
✅ **Workflow Files**: Copied to `.github/workflows/`

## Files Ready to Push

```
Script-Smiths/
├── .github/
│   └── workflows/
│       ├── ci.yml              ✅ CI Pipeline
│       └── terraform.yml       ✅ CD Pipeline
└── strapi/examples/getstarted/
    ├── Dockerfile.ci           ✅ Simplified Docker image
    ├── TASK6_README.md         ✅ Documentation
    └── terraform/
        ├── main.tf             ✅ Infrastructure config
        ├── variables.tf        ✅ Variables
        ├── outputs.tf          ✅ Outputs
        ├── user_data.sh        ✅ EC2 startup script
        └── ecr-policy.json     ✅ ECR public policy
```

## Before Pushing - Configure GitHub Secrets

You need to add AWS credentials to GitHub:

### Step 1: Go to GitHub Repository Settings
1. Navigate to: https://github.com/PearlThoughtsInternship/Script-Smiths
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**

### Step 2: Add These Secrets

**Secret 1: AWS_ACCESS_KEY_ID**
- Name: `AWS_ACCESS_KEY_ID`
- Value: Your AWS access key (from AWS IAM)

**Secret 2: AWS_SECRET_ACCESS_KEY**
- Name: `AWS_SECRET_ACCESS_KEY`
- Value: Your AWS secret key (from AWS IAM)

### How to Get AWS Credentials

If you don't have them:
```bash
# Check your current credentials
aws configure list

# Or create new IAM user with these permissions:
# - AmazonEC2FullAccess
# - AmazonECRFullAccess
```

## After Pushing - How to Use

### Automatic CI (Docker Build)

When you push code to `main` or `Arman_Bisht_v2` branch:
1. GitHub Actions automatically triggers
2. Builds Docker image from `Dockerfile.ci`
3. Pushes to ECR with tags: `latest` and `<commit-sha>`

### Manual CD (Terraform Deploy)

To deploy infrastructure:
1. Go to GitHub → **Actions** tab
2. Select **"CD - Terraform Deployment"**
3. Click **"Run workflow"**
4. Choose action:
   - `plan` - Preview changes
   - `apply` - Deploy to AWS
   - `destroy` - Remove infrastructure
5. Click **"Run workflow"**

## Current Infrastructure Status

Your EC2 instance is currently running:
- **Instance ID**: i-0e5d07d310efba871
- **Public IP**: 13.201.137.195
- **Strapi URL**: http://13.201.137.195:1337

## What Happens After Push

1. **CI Workflow** will trigger on push
2. It will build and push Docker image to ECR
3. You can then manually trigger **CD Workflow** to redeploy with new image

## Testing the Workflows

### Test CI Workflow
```bash
# Make a small change and push
git add .
git commit -m "Test CI workflow"
git push origin Arman_Bisht_v2
```

Then check: https://github.com/PearlThoughtsInternship/Script-Smiths/actions

### Test CD Workflow
1. Go to Actions tab
2. Select "CD - Terraform Deployment"
3. Run with `plan` first to preview
4. Then run with `apply` to deploy

## Ready to Push?

Run these commands:
```bash
cd Script-Smiths
git add .github/workflows/
git add strapi/examples/getstarted/Dockerfile.ci
git add strapi/examples/getstarted/TASK6_README.md
git add TASK6_SETUP.md
git commit -m "Add GitHub Actions CI/CD pipelines for Task 6"
git push origin Arman_Bisht_v2
```

## Important Notes

⚠️ **Before first workflow run**: Add AWS credentials to GitHub Secrets  
⚠️ **ECR Repository**: Already exists and configured with public access  
⚠️ **Current Instance**: Will remain running until you run `terraform destroy`  
⚠️ **Costs**: EC2 t3.micro is Free Tier eligible (750 hours/month)

## Troubleshooting

**If CI fails with "ECR login failed"**:
- Check AWS credentials in GitHub Secrets
- Verify IAM user has ECR permissions

**If CD fails with "Terraform init failed"**:
- Check AWS credentials
- Verify Terraform state is accessible

**If deployment succeeds but Strapi not accessible**:
- Wait 5-10 minutes for user_data script
- Check security group allows port 1337
- Verify instance is running in AWS Console
