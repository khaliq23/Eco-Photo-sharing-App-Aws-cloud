AWS : ECO-PHOTO SHARING-APP

Overview

This project demonstrates how to build and deploy an Eco-Photo Sharing Application using core AWS services through the AWS Management Console. It provides hands-on experience with AWS fundamentals — including compute, storage, networking, IAM, and monitoring. The app allows users to upload and view photos related to sustainability activities such as recycling, tree planting, and environmental awareness.

AWS Services Used

Amazon EC2 – Runs a Node.js backend server for handling image uploads

Amazon S3 – Hosts a static website frontend and stores uploaded photos

AWS IAM – Manages secure access between EC2 and S3 using roles and policies

Amazon VPC – Provides a secure network for EC2 instance deployment

Amazon CloudWatch – Monitors EC2 instance performance and sets up alerts

SETUP - PHOTO SHARING APP

Step 1 — AWS environment & billing

1. Sign in to AWS Console.

2. Choose a Region (e.g., *us-east-1*).

3. Billing → Cost Explorer → enable Free Tier alerts.

Step 2 — VPC & networking

1. VPC → *Create VPC*: EcoPhotoVPC CIDR 10.0.0.0/16.

2. Subnet → *Create subnet*: EcoPhotoSubnet 10.0.1.0/24, enable public IPv4.

3. Internet Gateway → create EcoPhotoIGW, attach to VPC.

4. Route table → add 0.0.0.0/0 → EcoPhotoIGW.

5. Security Group → EcoPhotoSG: inbound SSH (22) from My IP, TCP 3000 from 0.0.0.0/0.

Step 3 — S3 buckets

Create two globally-unique buckets in the same Region: -> eco-photos-rahul-005 (photo storage) — enable *Versioning*. -> eco-website-rahul-005 (static site) — enable *Static website hosting* (index: index.html).

*Bucket policy (photos)*: The screenshots are attached to folders

step 4- IAM roles for EC2

IAM -> Roles -> Create Roles -> EC2.
2. Create policy allowing S3 put/get for photo bucket.

Attach policy to role \*EcoPhotoRole\*
Step 5 -Launch EC2 & Backend

EC2 → Launch instance:
-> AMI: Amazon Linux 2 -> Type: t2.micro -> Name: EcoPhotoServer -> IAM role: EcoPhotoRole -> Key pair: create/download eco-photo-key.pem -> Network: EcoPhotoVPC, EcoPhotoSubnet, EcoPhotoSG

2.Connect to EC2:

--> setup putty :

* IP address, SSH * Set connections > Auth > credentials > covert the key.pem into key.ppk. * Login as ec2 user on Linux environment.

Setup Node.js backend
-> Update the instance: sudo yum update -y. -> Install Node.js: sudo yum install -y nodejs npm. -> Create app.js (\*the json format images are uploaded in separate files)

Step 6 — Frontend & Deployment

Create index.html. (*the Vs code html format uploaded in separate files.)

Upload to website bucket:

-> aws s3 cp index.html s3://eco-website-rahul-005/\*

Open S3 website endpoint to view.

Step 7 — Monitoring & security

CloudWatch → Alarms → Create alarm → Metric: EC2 CPU Utilization for EcoPhotoServer → Alert if >80% for 5 min.

S3 photo bucket → Properties → Default encryption → SSE-S3.

Confirm EC2 uses IAM role (no keys on instance).

Step 8 — Test

From local:
->curl -F "photo=@./myphoto.jpg" http://:3000/upload

Visit site URL, confirm uploaded image appears in gallery.
step 9 — Clearing

-> Terminate and delete all the :

* Ec2 * s3 * VPC * Cloud watch

Outcomes

Successfully deployed a photo-sharing web application integrating EC2 and S3.

Learned how to connect AWS services securely using IAM roles and policies.

Practiced managing EC2 networking, storage configuration, and performance monitoring.

Gained hands-on experience with AWS Free Tier services and cloud best practices.
