# 🚀 Automated Static Website Deployment to AWS S3 using GitHub Actions  

A complete CI/CD setup for static websites (HTML/CSS/JS) with GitHub Actions and AWS S3.  
This guide helps you automate deployments every time you push code to the `master` branch.

---

## 📌 Features

✅ Zero-cost hosting (S3 free tier eligible)  
✅ Instant deployments on code push to master branch  
✅ Manual trigger support via GitHub UI  
✅ Secure AWS access using IAM roles  

---

## 🛠️ Prerequisites

- AWS account with S3 access  
- GitHub repository  
- Basic knowledge of Git  

---

## ⚡ Quick Setup

### 1. Configure AWS S3 Bucket

aws s3api create-bucket --bucket YOUR-BUCKET-NAME --region us-west-2 --acl public-read
aws s3 website s3://YOUR-BUCKET-NAME/ --index-document index.html


## 🔐 2. Set Up GitHub Secrets

Go to your repository →  
**Settings → Secrets → Actions → New repository secret**

Add the following secrets:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_S3_BUCKET` *(your bucket name)*

---

## 📝 3. Add GitHub Actions Workflow File

Create a file at: `.github/workflows/deploy.yml`  
Paste the following configuration:

name: Deploy to S3

on:
  push:
    branches: [ master ]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      - run: aws s3 sync ./ s3://${{ secrets.AWS_S3_BUCKET }}/ --delete

## 📁 Project Structure


├── .github/
│   └── workflows/
│       └── deploy.yml       # GitHub Actions config
├── index.html               # Website entry point
├── style.css                # Styles
└── script.js                # JavaScript

## 🚨 Troubleshooting

- **403 Errors**: Verify your S3 bucket policy and IAM permissions are correctly set.
- **Workflow Fails**: Check the logs in GitHub Actions for detailed error messages.
- **Files Not Updating**: Make sure the `--delete` flag is used in the `aws s3 sync` command to remove outdated files.

---

## 📜 License

MIT © MarjanRafi  
Feel free to fork and customize!
Video link: https://www.linkedin.com/in/mdmarjanmorshedrafi/
