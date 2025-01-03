![Alt text](/Host_a_Static_Website_on_AWS.png)


# Host a Static Website on AWS

This project demonstrates how to host a static HTML web application on AWS using various AWS resources. The goal was to create a fault-tolerant, scalable, and highly available infrastructure to serve a static website. 

## Resources and Services Used

1. **Virtual Private Cloud (VPC):** 
   - Configured a VPC with both public and private subnets across two availability zones (AZs) for high availability.
   - Deployed an **Internet Gateway** (IGW) for connectivity between the VPC and the internet.

2. **Security Groups:** 
   - Used Security Groups as a network firewall to control inbound and outbound traffic to EC2 instances.

3. **Availability Zones:**
   - Utilized multiple Availability Zones (AZs) to increase fault tolerance and ensure the application is highly available.

4. **Subnets:**
   - Deployed infrastructure components (e.g., NAT Gateway, Application Load Balancer) in **Public Subnets**.
   - Positioned **Web Servers (EC2 Instances)** in **Private Subnets** for better security.

5. **NAT Gateway:**
   - Ensured EC2 instances in private subnets could access the internet for tasks like software updates and external API calls.

6. **EC2 Instances and Web Hosting:**
   - Hosted the static website on EC2 instances.
   - Used **EC2 Instance Connect Endpoint** for secure access to EC2 instances in private subnets.

7. **Application Load Balancer (ALB):**
   - Employed an **Application Load Balancer (ALB)** to evenly distribute web traffic across EC2 instances in multiple AZs.
   - Configured a **Target Group** to register EC2 instances and ensure high availability.

8. **Auto Scaling Group:**
   - Set up an **Auto Scaling Group** (ASG) to automatically adjust the number of EC2 instances based on traffic, ensuring availability, scalability, and fault tolerance.

9. **GitHub for Version Control:**
   - Hosted web files on **GitHub** for version control and collaboration.

10. **SSL/TLS Encryption:**
   - Used **AWS Certificate Manager (ACM)** to secure communications between users and the web server.

11. **Simple Notification Service (SNS):**
   - Configured **SNS** to send alerts regarding Auto Scaling Group activities and any other important events.

12. **Route 53:**
   - Registered the domain name and set up **DNS** records for routing traffic to the hosted website.

## Architecture Diagram

![Architecture Diagram](URL_to_your_image)

## Installation Instructions

### Prerequisites

1. **AWS Account**: An active AWS account with necessary IAM permissions.
2. **Domain Name**: Registered domain name for hosting.
3. **GitHub**: The GitHub repository to host the web files.

### EC2 Instance Setup

1. SSH into your EC2 instance or use EC2 Instance Connect.
2. Once connected, run the following script to install the necessary software and deploy the static website:

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

### DNS Configuration

1. Use **Amazon Route 53** to configure DNS records for your domain.
2. Point your domain to the **Elastic IP** or **Load Balancer DNS** provided by AWS.

### Auto Scaling Group Configuration

1. Define the Auto Scaling policies in AWS to automatically scale based on traffic demands.
2. Configure your Application Load Balancer with a target group that registers the EC2 instances.

### SSL/TLS Configuration

1. Use **AWS Certificate Manager (ACM)** to request a free SSL certificate for your domain.
2. Attach the certificate to your Application Load Balancer for encrypted HTTPS traffic.

### Monitoring and Alerts

1. Use **Amazon SNS** to receive alerts about Auto Scaling Group activities and other events.
2. Configure **CloudWatch** for detailed monitoring of EC2 instances and ALB.

## Project Folder Structure

```plaintext
.
├── .git/
├── index.html
├── styles.css
└── scripts.js
```

### GitHub Repository

For more information and to download the full code, visit the [GitHub repository](https://github.com/aosnotes77/host-a-static-website-on-aws).

## Contributing

If you want to contribute to this project, feel free to fork the repository, make changes, and submit pull requests.

## License

This project is licensed under the MIT License.

---

