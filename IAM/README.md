# AWS IAM Practice

## Overview

This project documents my hands-on practice with **AWS Identity and Access Management (IAM)**. I practiced IAM users, policies, groups, roles, permissions, trust policies, and permission testing.

## IAM Concepts Practiced

- IAM Users
- IAM Policies
- IAM Permissions
- IAM Groups
- IAM Roles
- Trust Policies
- Least Privilege
- Permission Testing

## 1. IAM User

Created an IAM user named:

`fkdemo`

Configured console access and tested login using a separate browser.

Screenshot:** `IAM-User-Creation.png`  
Screenshot: `IAM-User-Login.png`  
Screenshot: `IAM-User-Logged-In.png`

## 2. IAM Policies & Permissions

Attached the AWS-managed policy:

`AmazonS3ReadOnlyAccess`

Tested the permission by accessing S3. The user could access S3 but could not create a bucket.

Screenshot: `IAM-Policy-Attached.png`  
Screenshot:`IAM-S3-Access-Denied.png`

## 3. IAM Groups

Created the group:

`demogrp`

Added `fkdemo` to the group and configured S3 read-only permissions.

Screenshot: `IAM-Group-Created.png`  
Screenshot: `IAM-Group-User-Added.png`

## 4. IAM Role

Created:

`EC2-S3-ReadOnly-Role`

Configured **EC2** as the trusted entity and attached `AmazonS3ReadOnlyAccess`.

```text
EC2
 ↓
IAM Role
 ↓
S3 ReadOnly Policy
 ↓
Amazon S3


5. Key Learnings
IAM controls access to AWS resources.
Policies define permissions.
Groups manage permissions for multiple users.
Roles provide permissions to trusted services.
Trust policies define who can assume a role.
Least privilege helps avoid unnecessary access.
IAM permissions can be tested using real AWS actions.


6. Cleanup
After completing the practice, the temporary resources were deleted:

fkdemo
demogrp
EC2-S3-ReadOnly-Role

The AWS-managed AmazonS3ReadOnlyAccess policy was kept.