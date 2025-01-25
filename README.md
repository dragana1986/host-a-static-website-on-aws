![Alt text](/Host_a_Static_Website_on_AWS.png)

# Hosting a Static Website on AWS

This project demonstrates how to host a static HTML web app on AWS using various AWS resources such as EC2, VPC, Auto Scaling, and more. The web app is deployed on an EC2 instance behind an Application Load Balancer for high availability and scalability.

## Resources Utilized

- **VPC (Virtual Private Cloud)** with public and private subnets across two different availability zones.
- **Internet Gateway** to facilitate connectivity between instances in the VPC and the internet.
- **Security Groups** to act as a network firewall.
- **Public Subnets** for resources like NAT Gateway and Application Load Balancer.
- **Private Subnets** for web servers (EC2 instances) to enhance security.
- **EC2 Instances** to host the static website.
- **Auto Scaling Group** to ensure website scalability and fault tolerance.
- **Application Load Balancer** to distribute web traffic evenly across EC2 instances.
- **AWS Certificate Manager** to secure application communications.
- **SNS (Simple Notification Service)** to send alerts about Auto Scaling Group activities.
- **Route 53** for DNS management and domain registration.
  
## Deployment Steps

1. **VPC Configuration**: 
   - A VPC was configured with both public and private subnets across two availability zones.
   - An Internet Gateway was deployed for external connectivity.
   
2. **Security Setup**: 
   - Security Groups were created to define inbound and outbound traffic rules for the EC2 instances.
   
3. **EC2 Instances Setup**: 
   - EC2 instances were launched within private subnets to host the static website.
   - EC2 Instance Connect Endpoint was configured to enable secure connections.
   
4. **Load Balancing & Scaling**:
   - An Application Load Balancer was deployed in the public subnet to evenly distribute web traffic to EC2 instances.
   - Auto Scaling was configured to automatically scale EC2 instances based on traffic demand.
   
5. **Web Hosting**: 
   - Apache HTTP Server was installed on the EC2 instances to serve the static website.
   - Web files were cloned from a GitHub repository and deployed on the web server.

6. **Domain and DNS**: 
   - The domain was registered and DNS settings were configured in AWS Route 53.

## Infrastructure Diagram

The reference diagram of the architecture and related scripts for this project are available in the [GitHub Repository](https://github.com/aosnotes77/host-a-static-website-on-aws).

## Installation and Setup

Follow these steps to set up the environment and deploy the static website:

1. **EC2 Instance Setup**:
   - Launch an EC2 instance in your VPC and configure it to use a public subnet.
   - SSH into the EC2 instance and run the following script to deploy the web app:
   
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

2. **Verify the Web App**:
   - Once the script has been executed, the static website should be available by visiting the public IP of the EC2 instance or the domain name if Route 53 is configured.

## Features

- **Scalability**: The Auto Scaling Group ensures that your web servers scale based on the traffic demand.
- **High Availability**: Deployed in multiple availability zones to prevent downtime in case of failures.
- **Security**: EC2 instances are hosted in private subnets, with traffic only routed through the Load Balancer.
- **HTTPS**: Communications between the clients and web servers are encrypted using SSL/TLS certificates provided by AWS Certificate Manager.

## Prerequisites

- An AWS account.
- A registered domain name (optional if using Route 53).
- Basic knowledge of AWS services such as EC2, VPC, Route 53, and Auto Scaling.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

For any questions or feedback, feel free to reach out to me via my GitHub repository: [https://github.com/aosnotes77](https://github.com/aosnotes77).

---

Let me know if you'd like to tweak or expand on any of the sections!
