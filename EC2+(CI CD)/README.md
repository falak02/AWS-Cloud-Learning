# AWS EC2 + CI/CD Portfolio Deployment

## Project Overview

This project demonstrates how I deployed my personal portfolio website on an AWS EC2 server and automated the deployment process using GitHub Actions.

The project covers EC2 setup, Apache web server configuration, Security Groups, SSH, SCP, GitHub, GitHub Actions, and CI/CD.

---

## 1. AWS EC2 Setup

Amazon EC2 was used as the cloud server for hosting the portfolio website.

### EC2 Configuration

- OS: Ubuntu Server 24.04 LTS
- Instance Type: t3.micro
- Region: Europe (Stockholm)
- Availability Zone: eu-north-1b
- Web Server: Apache

The EC2 instance was launched inside an AWS VPC and subnet.

---

## 2. Security Group

The EC2 Security Group was configured to allow the following traffic:

| Type | Port | Source | Purpose |
|---|---:|---|---|
| HTTP | 80 | 0.0.0.0/0 | Website access |
| SSH | 22 | My IP | Secure server access |

For CI/CD testing, SSH port 22 was temporarily changed to:

```text
0.0.0.0/0

This allowed the GitHub Actions runner to connect to the EC2 instance.

After the CI/CD test was completed successfully, SSH access was changed back to My IP.

## Installing Apache
 Apache was used as the web server to host the portfolio.

##Testing Apache
After installing Apache, the EC2 public IP was opened in a browser.
The default Apache page was displayed.

This confirmed that:
EC2 was running
Apache was running
Port 80 was accessible
The Security Group was configured correctly
The EC2 server could be accessed through the internet

Manual Deployment Process

Before implementing CI/CD, the portfolio was deployed manually to confirm that EC2 and Apache were working correctly.

Local Computer
      |
      | SCP
      v
AWS EC2
      |
      v
/tmp/portfolio/
      |
      v
/var/www/html/
      |
      v
Apache
      |
      v
Portfolio Website



## CI/CD Workflow

I created the workflow file inside:

.github/workflows/portfolio-ci.yml

The workflow performs these tasks:

Gets the latest code from GitHub.
Checks that the required portfolio files are present.
Uses the SSH key stored in GitHub Secrets.
Connects to the EC2 server.
Creates a temporary folder on EC2.
Copies the updated portfolio files using SCP.
Updates the Apache web directory.
Reports whether the deployment was successful.
GitHub Actions Workflow
name: Portfolio CI/CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:

      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Check portfolio files
        run: |
          test -f index.html
          test -f styles.css
          test -f script.js

      - name: Setup SSH key
        run: |
          printf '%s\n' "${{ secrets.EC2_SSH_KEY }}" > ec2-key.pem
          chmod 600 ec2-key.pem

      - name: Prepare EC2
        run: |
          ssh -o StrictHostKeyChecking=no -i ec2-key.pem \
          ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }} \
          "mkdir -p /tmp/portfolio"

      - name: Deploy files to EC2
        run: |
          scp -o StrictHostKeyChecking=no -i ec2-key.pem -r \
          index.html styles.css script.js assets \
          ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }}:/tmp/portfolio/

      - name: Update Apache
        run: |
          ssh -o StrictHostKeyChecking=no -i ec2-key.pem \
          ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }} \
          "sudo rm -rf /var/www/html/* && sudo cp -r /tmp/portfolio/* /var/www/html/"

      - name: Deployment successful
        run: echo "Portfolio deployed successfully to EC2!"



## GitHub Repository Secrets

To allow GitHub Actions to connect to my EC2 server securely, I stored the required information in GitHub Repository Secrets.

I used:

Secret	Purpose
EC2_HOST	EC2 public IP address
EC2_USER	EC2 username
EC2_SSH_KEY	EC2 private SSH key

The private SSH key was not written directly inside the workflow file.

Instead, it was stored as a GitHub Secret and accessed by the workflow when required.


## Complete EC2 + CI/CD Architecture


                         Developer
                             |
                             v
                          VS Code
                             |
                             | git push
                             v
                     GitHub Repository
                             |
                             v
                     GitHub Actions
                             |
                +------------+------------+
                |                         |
                v                         v
         CI: Check Files          CD: Deployment
                                          |
                                          v
                                     SSH to EC2
                                          |
                                          v
                                     SCP Files
                                          |
                                          v
                                /var/www/html/
                                          |
                                          v
                                       Apache
                                          |
                                          v
                                 Portfolio Website
                                 
                                 
## What I Learned

Through this project, I learned how to:

Launch and configure an EC2 instance.
Work with an Ubuntu cloud server.
Configure AWS Security Groups.
Connect to EC2 using SSH.
Install and configure Apache.
Host a website on AWS.
Transfer files using SCP.
Manage code using Git and GitHub.
Create GitHub Actions workflows.
Implement CI/CD.
Automate deployment from GitHub to EC2.
Use GitHub Secrets for sensitive information.
Apply basic cloud security practices.

