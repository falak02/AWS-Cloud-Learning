# Amazon S3

## Overview

Amazon Simple Storage Service (S3) is an object storage service used to store and retrieve files and data.
I used Amazon S3 to understand how buckets, objects, permissions, bucket policies, and static website hosting work in practice.

## Concepts Covered
* S3 buckets
* Objects
* AWS Regions
* Uploading files
* Bucket policies
* Public access
* Static website hosting
* Index documents
* S3 website endpoints

## Hands-on Practice

As part of my hands-on practice, I created an S3 bucket and used it to deploy a static web application.

### Static Website Deployment

I deployed my **Planet Weight Calculator** application using Amazon S3 Static Website Hosting.

The application consists of:

* HTML
* CSS
* JavaScript
* Image assets

### Deployment Process

The deployment was completed through the following steps:

```text
Local Web Application
        ↓
Create S3 Bucket
        ↓
Upload Website Files
        ↓
Configure Static Website Hosting
        ↓
Set index.html
        ↓
Configure Bucket Policy
        ↓
Access S3 Website Endpoint
        ↓
Live Web Application
```

## 1. Creating the S3 Bucket
I created a dedicated S3 bucket for the Planet Weight Calculator and selected the Mumbai AWS Region (`ap-south-1`).

![S3 Bucket](screenshots/s3-bucket.png)

## 2. Uploading Files
I uploaded the files required for the application to run:

* `index.html`
* `index.css`
* `main.js`
* `6.png`

![Uploaded Files](screenshots/uploaded-files.png)

## 3. Enabling Static Website Hosting
I enabled Static Website Hosting for the bucket and configured `index.html` as the index document.

![Static Website Hosting](screenshots/static-hosting.png)

## 4. Configuring Bucket Policy
I configured an S3 bucket policy to allow users to retrieve the website objects.

The policy uses the `s3:GetObject` permission so that the browser can access the HTML, CSS, JavaScript, and image files required by the application.

## 5. Accessing the Live Website
After completing the configuration, I accessed the application through the S3 website endpoint.

![Live Website](screenshots/live-website.png)

## Project

### Planet Weight Calculator

The Planet Weight Calculator is a static web application that calculates a person's weight on different planets.

The complete project, including the application source code, screenshots, and demo, is available in my separate project repository.

## What I Learned

This hands-on exercise helped me understand the complete process of deploying a static website using Amazon S3.
I learned how to create and configure a bucket, upload website files, enable static website hosting, configure access using a bucket policy, and access the deployed application through an S3 website endpoint.
