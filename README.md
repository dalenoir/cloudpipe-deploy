# cloudpipe-deploy
# cloudpipe-deploy

Automated CI/CD pipeline that deploys a static website to AWS S3 and serves it through CloudFront.  
Every push to `main` triggers GitHub Actions to sync site files to S3 and invalidate the CloudFront cache.

## Architecture

GitHub Actions → S3 (private bucket) → CloudFront (CDN) + cache invalidation

**Key pieces:**
- **Terraform** provisions S3 + CloudFront (with Origin Access Control)
- **GitHub Actions** deploys static site files on every push
- **CloudFront invalidation** ensures updates appear immediately

## Live Demo

- CloudFront URL: `https://d2yi86ecg51ji.cloudfront.net/`

## Repo Structure

- `website/` — static site files (HTML/CSS/JS)
- `terraform/` — infrastructure as code
- `.github/workflows/deploy.yml` — CI/CD workflow

## Prerequisites

- AWS account + IAM user/role with permissions for:
  - `s3:ListBucket`, `s3:PutObject`, `s3:DeleteObject`
  - `cloudfront:CreateInvalidation`
- Terraform installed
- GitHub repository secrets configured

## GitHub Secrets Required

Set these in: **Repo → Settings → Secrets and variables → Actions**
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`

## Deploy Infrastructure (Terraform)

```bash
cd terraform
terraform init
terraform apply
