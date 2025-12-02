# S3 Static Restaurant Menu Site

This repository contains a static restaurant menu website built with HTML and CSS and hosted on Amazon S3 using the AWS CLI.

## Project Overview

- Static frontend: HTML/CSS restaurant menu page.
- Deployed as an S3 static website.
- All configuration done from the terminal using AWS CLI commands (bucket creation, website hosting, public access configuration, and file upload).

## Structure

- `index.html` – main restaurant menu page.
- `style.css` – styles for the menu layout and design.
- `public-access.json` – configuration for S3 Block Public Access settings.
- `bucket-policy.json` – S3 bucket policy allowing public read access to website objects.

## Deployment Steps (AWS CLI)

### 1. S3 bucket creation

aws s3 mb s3://ahmedelfar6811 --region us-east-1

### 2. enabling static website hosting

aws s3 website s3://ahmedelfar6811 --index-document index.html

### 3. Uploading website files

aws s3 cp . s3://ahmedelfar6811 --recursive

### 4. Block Public Access configuration

`public-access.json`:

{
"BlockPublicAcls": false,
"IgnorePublicAcls": false,
"BlockPublicPolicy": false,
"RestrictPublicBuckets": false
}

Apply configuration:

aws s3api put-public-access-block --bucket ahmedelfar6811 --public-access-block-configuration file://public-access.json

### 5. bucket policy configuration for public read

`bucket-policy.json`:

{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "AllowPublicReadForWebsite",
"Effect": "Allow",
"Principal": "",
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::ahmedelfar6811/"
}
]
}

Apply policy:

aws s3api put-bucket-policy --bucket ahmedelfar6811 --policy file://bucket-policy.json

## Result

- The restaurant menu site is accessible via the S3 static website endpoint for the bucket.
- All hosting and permissions are managed via AWS CLI only.
