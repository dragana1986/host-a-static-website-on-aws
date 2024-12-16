

# README for Hosting a Static Website on AWS

## Project Overview
This project demonstrates how to host a static HTML web application on AWS (Amazon Web Services). It leverages various AWS services like EC2, VPC, Auto Scaling Groups, Application Load Balancers, Route 53, and more to create a highly available, scalable, and secure environment for the website. The setup includes configuring infrastructure, deploying web servers, managing traffic distribution, and securing application communications.

## Architecture

The architecture for hosting the static website on AWS includes the following components:

1. **Virtual Private Cloud (VPC)**: A VPC was created with both public and private subnets across two availability zones to ensure redundancy and fault tolerance.
2. **Internet Gateway**: Deployed to allow connectivity between VPC instances and the wider internet.
3. **Security Groups**: Set up as a network firewall mechanism to control inbound and outbound traffic to EC2 instances.
4. **Application Load Balancer (ALB)**: Used to distribute web traffic evenly across EC2 instances in multiple availability zones.
5. **Auto Scaling Group (ASG)**: Automatically manages EC2 instances to ensure website availability, scalability, and fault tolerance.
6. **NAT Gateway**: Enables instances in private subnets to access the internet for updates and external communication.
7. **EC2 Instances**: Hosts the static website content and serves web pages to users.
8. **Certificate Manager**: Used to secure communications between users and the web application.
9. **Simple Notification Service (SNS)**: Alerts for activities within the Auto Scaling Group.
10. **Route 53**: Manages DNS records for the domain name registration and resolution.

## Resources Used

- **EC2 Instances** for web server hosting.
- **VPC** with public and private subnets across two availability zones.
- **Security Groups** for network security and access control.
- **NAT Gateway** for private subnet internet access.
- **Application Load Balancer (ALB)** for traffic distribution.
- **Auto Scaling Group** for EC2 instance management.
- **Route 53** for DNS and domain name management.
- **AWS Certificate Manager** for SSL/TLS encryption.
- **SNS** for notifications about Auto Scaling activities.

## Setup Instructions

Follow the instructions below to deploy the static website on AWS:

### 1. Create a VPC and Subnets
- Create a Virtual Private Cloud (VPC) with both public and private subnets.
- Ensure the public subnets are associated with an internet gateway to provide internet access.

### 2. Set Up Security Groups
- Create security groups for EC2 instances to restrict inbound and outbound traffic based on the required application ports.

### 3. Deploy an EC2 Instance
- Launch EC2 instances in the private subnets to host the web application.
- SSH into the EC2 instances to configure the Apache HTTP server.

### 4. Set Up Application Load Balancer
- Create an Application Load Balancer (ALB) to distribute web traffic evenly to the EC2 instances in multiple availability zones.
- Set up the target group to include the EC2 instances as targets.

### 5. Enable Auto Scaling
- Create an Auto Scaling Group (ASG) to automatically manage EC2 instances and ensure scalability and fault tolerance.
- Configure scaling policies to handle changes in traffic.

### 6. Install Web Server and Git
Use the following script to install and configure the Apache HTTP Server on your EC2 instance and clone the web application from GitHub.

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

### 7. Set Up the DNS and SSL
- Use AWS Route 53 to register your domain and configure DNS settings to point to your ALB.
- Set up an SSL certificate using AWS Certificate Manager to secure the website.

### 8. Monitor and Notify
- Set up SNS to monitor activities within your Auto Scaling Group, such as scaling events and instance health checks.

## GitHub Repository

The full project code and infrastructure setup scripts are available on the [GitHub repository](https://github.com/aosnotes77/host-a-static-website-on-aws).

## Conclusion

By following these steps, you'll be able to successfully deploy a highly available and scalable static website on AWS using EC2, VPC, Auto Scaling, ALB, Route 53, and other AWS services. The architecture ensures fault tolerance, scalability, and security for your application.
