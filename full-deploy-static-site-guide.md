
# 🚀 How to Deploy a Static Website to an S3 Bucket Using GitHub Actions

In this article, we are going to deploy a static website to Amazon S3 automatically using GitHub Actions anytime you push your changes to your repo.

---

## ✅ STEP 1: Your Base Project

a) If you have your own HTML template, you can use it. For this article, we are using a pre-built static website template from [Free CSS](https://www.free-css.com/).  
b) Once downloaded, extract the archive. Create a folder named `public` and move the project files to it.  
c) Create a new folder and name it whatever you want, then move the `public` folder into this new folder.

---

## ✅ STEP 2: Create an S3 Bucket and Configure Static Hosting

Go to the official [Amazon S3 documentation on configuring static websites](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html).  
Complete only **Step 1 to Step 4** of the guide and skip Step 5 onward.

---

## ✅ STEP 3: Create a New Repository in Your GitHub Account

We need to configure your GitHub repository with AWS secrets.

a) Go to GitHub and create a new repository.  
b) Enter your repository name and click **Create repository**.  
c) On your new repository, go to **Settings**.  
d) Select **Secrets and Variables** > **Actions**.  
e) Click **New repository secret**.  
f) Add:
   - **Name:** `AWS_ACCESS_KEY_ID`
   - **Value:** Your AWS access key  
g) Click **Add secret**.  
h) Add another:
   - **Name:** `AWS_SECRET_ACCESS_KEY`
   - **Value:** Your AWS secret key  
i) Verify that both secrets are now listed.

---

## ✅ STEP 4: Create GitHub Actions Workflow

a) Open the project root directory in your preferred text editor.  
b) Create the following folder structure:

```
.github/workflows/main.yml
```

c) Add the following content to `main.yml`:

```yaml
name: Upload Website

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Deploy static site to S3 bucket
        run: aws s3 sync ./public/ s3://BUCKET_NAME --delete
```

> ✏️ Replace `BUCKET_NAME` with your actual AWS S3 bucket name and adjust `aws-region` if needed.

This workflow triggers on every push to the `main` branch, checks out the code, configures AWS credentials, and syncs the `public` folder with your S3 bucket.

---

## ✅ STEP 5: Push Your Code to GitHub

Open Git Bash or a terminal in your project root, and run the following commands:

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin <your-git-repo-url>
git branch -M main
git push -u origin main
```

Then, go to your GitHub repository and check the **Actions** tab to monitor workflow status.

---

## ✅ STEP 6: Test Your Static Website

a) Sign in to the AWS Management Console and open the **Amazon S3** console.  
b) In the bucket list, choose the bucket where you enabled static website hosting.  
c) Go to **Properties**.  
d) Under **Static website hosting**, click the **Endpoint URL** to view your live website.

---

🎉 **Congratulations!** Your static site is now hosted on Amazon S3 and automatically deploys with every push to the `main` branch.
