


# Host a Static Website on AWS - Project README

This repository contains the implementation details and scripts used to deploy a static HTML web application on Amazon Web Services (AWS). The goal of this project was to host a simple, static website on AWS using several key resources and best practices to ensure scalability, security, and high availability.

## Architecture Overview

The project leverages multiple AWS services to host a highly available and scalable static website. Below is a breakdown of the key components used in the infrastructure:

1. **Virtual Private Cloud (VPC)**: 
   - Configured a VPC with both **public** and **private** subnets across two different Availability Zones (AZs) to ensure redundancy and fault tolerance.
   
2. **Internet Gateway**: 
   - Deployed an Internet Gateway to allow instances in the public subnet to access the internet and communicate with the wider internet.
   
3. **Security Groups**: 
   - Established Security Groups to act as a network firewall, controlling inbound and outbound traffic to and from EC2 instances.
   
4. **Auto Scaling Group (ASG)**: 
   - Used an Auto Scaling Group to automatically scale EC2 instances based on demand, ensuring high availability and elasticity for the website.
   
5. **Application Load Balancer (ALB)**: 
   - Deployed an Application Load Balancer to evenly distribute incoming web traffic to multiple EC2 instances across the Availability Zones.
   
6. **EC2 Instances**: 
   - Hosted the static website on EC2 instances, positioned in private subnets for security purposes. These instances can access the internet via a NAT Gateway in the public subnet.

7. **NAT Gateway**: 
   - Configured a NAT Gateway to allow EC2 instances in private subnets to securely access the internet (for updates, downloads, etc.) without exposing them to public internet access.
   
8. **AWS Certificate Manager (ACM)**: 
   - Secured the application using SSL/TLS certificates to ensure encrypted communication over HTTPS.
   
9. **Amazon Route 53**: 
   - Registered the domain name and set up DNS records for routing traffic to the Application Load Balancer.
   
10. **Amazon SNS**: 
   - Configured Simple Notification Service (SNS) to send alerts about activities within the Auto Scaling Group, such as instance health changes or scaling events.
   
11. **GitHub Repository**: 
   - Stored the website files in a GitHub repository for version control and collaboration.

## Prerequisites

- An AWS account with appropriate permissions to create and manage resources like VPC, EC2, Route 53, ACM, SNS, etc.
- GitHub repository containing the static HTML files.
- Domain registered (or use Route 53 to register a domain).
- AWS CLI installed for managing resources (optional, but recommended).

## Deployment Steps

### 1. Set Up the VPC, Subnets, and Networking
- Create a Virtual Private Cloud (VPC) with public and private subnets in two different Availability Zones (AZs).
- Deploy an **Internet Gateway** to allow outbound internet connectivity for public instances.
- Set up a **NAT Gateway** in the public subnet to enable private subnet instances to access the internet securely.
- Define **Security Groups** to manage inbound and outbound traffic to your EC2 instances.

### 2. Launch EC2 Instances
- Launch EC2 instances within the private subnets to host the website.
- Use the provided Bash script (below) to install and configure Apache HTTP Server and deploy the static website.

### 3. Configure Load Balancer and Auto Scaling
- Deploy an **Application Load Balancer** (ALB) in the public subnet to distribute web traffic across multiple EC2 instances in the Auto Scaling Group.
- Configure an **Auto Scaling Group** (ASG) to automatically scale your EC2 instances to handle varying web traffic loads.

### 4. Configure SSL with AWS Certificate Manager (ACM)
- Request an SSL certificate for your domain using **AWS Certificate Manager** (ACM).
- Attach the SSL certificate to your Application Load Balancer to secure web traffic over HTTPS.

### 5. Set Up Route 53
- Register a domain name using **Amazon Route 53** or use an existing domain.
- Create DNS records to point to the Application Load Balancer.

### 6. Configure SNS for Auto Scaling Alerts
- Set up **Simple Notification Service (SNS)** to receive alerts on Auto Scaling activities.

### 7. Deployment Script for EC2 Instances

Use the following Bash script to deploy Apache HTTP Server and clone the GitHub repository containing your website files to the EC2 instances:

```bash
#!/bin/bash
# Switch to the root user to gain full administrative privileges
sudo su

# Update all installed packages to their latest versions
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change the current working directory to the Apache web root
cd /var/www/html

# Install Git
yum install git -y

# Clone the project GitHub repository to the current directory
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git

# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/

# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws

# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd

# Start the Apache HTTP Server to serve web content
systemctl start httpd
```

### 8. Domain Setup and Routing
- Configure DNS records in Route 53 to point to your Application Load Balancer.
- Set up any necessary A or CNAME records for routing traffic to the ALB.

### 9. Monitor and Scale
- Monitor the Auto Scaling Group and receive alerts through SNS regarding any scaling activities.

## Repository Contents

- **CloudFormation Templates** (optional): For automating the creation of AWS resources.
- **Deployment Scripts**: The Bash script used to install and deploy the website on EC2 instances.
- **Website Files**: HTML, CSS, JavaScript, and other assets for the static website.
- **Documentation**: Diagrams, architectural details, and deployment instructions.

## Conclusion

This project demonstrates how to deploy a highly available and scalable static website on AWS using EC2, Load Balancers, Auto Scaling, and other AWS services. The deployment architecture ensures reliability, fault tolerance, and secure communication for web traffic. The use of GitHub for version control ensures that all changes to the website can be tracked and deployed efficiently.
