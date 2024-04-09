# AWS WordPress Deployment Project
![alt text]()

This project aims to deploy a WordPress website on Amazon Web Services (AWS) infrastructure with high availability, scalability, and fault tolerance. Below is a detailed guide on setting up the project and deploying the WordPress website on AWS.

## Architecture Overview

1. **Virtual Private Cloud (VPC)**:
   - Configured with public and private subnets across two availability zones.
   - Utilized for network isolation and segmentation.

2. **Internet Gateway**:
   - Deployed to facilitate connectivity between VPC instances and the wider Internet.

3. **Security Groups**:
   - Established as a network firewall mechanism for controlling inbound and outbound traffic.

4. **Availability Zones**:
   - Leveraged two Availability Zones to enhance system reliability and fault tolerance.

5. **Public Subnets**:
   - Utilized for infrastructure components like the NAT Gateway and Application Load Balancer.

6. **EC2 Instance Connect Endpoint**:
   - Implemented for secure connections to assets within both public and private subnets.

7. **Web Servers (EC2 instances)**:
   - Positioned within Private Subnets for enhanced security.

8. **NAT Gateway**:
   - Enabled instances in both private Application and Data subnets to access the Internet.

9. **Hosting the Website**:
   - Deployed WordPress on EC2 Instances.

10. **Application Load Balancer (ALB)**:
    - Employed for evenly distributing web traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.

11. **Auto Scaling Group**:
    - Utilized to automatically manage EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.

12. **Storage**:
    - Stored web files on GitHub for version control and collaboration.
    - Utilized Amazon EFS for a shared file system.

13. **Database**:
    - Used Amazon RDS for hosting the WordPress database.

14. **Certificate Manager**:
    - Secured application communications between users and websites.

15. **Simple Notification Service (SNS)**:
    - Configured to alert about activities within the Auto Scaling Group.

16. **DNS**:
    - Registered the domain name and set up DNS records using Route 53.

## Deployment Instructions

### 1. Setting up the Infrastructure
   - Configure the VPC, Internet Gateway, Security Groups, Subnets, and Route 53 DNS records.
   - Deploy NAT Gateway and ALB.
   - Set up EC2 instances and RDS for the database.

### 2. Installing WordPress
   - Execute the provided script to install WordPress on EC2 instances.
   - Mount EFS for shared file storage.
   - Configure Apache web server with PHP and necessary extensions.
   - Set up MySQL server and permissions.
   - Download and configure WordPress files.

### 3. Configuring Auto Scaling Group
   - Use the provided script to set up an Auto Scaling Group launch template.
   - Install necessary software packages on EC2 instances.
   - Mount EFS and configure permissions.
   - Configure Apache web server and MySQL server.

### 4. Lessons Learned
   - Referencing AWS documentation and stackoverflow during configuration troubleshooting.

## Contributors and References

- AWS docuemntation.
- Stackoverflow.

## Project Script
#!/bin/bash

# update the software packages on the ec2 instance 
sudo yum update -y

# install the apache web server, enable it to start on boot, and then start the server immediately
sudo yum install -y httpd
sudo systemctl enable httpd 
sudo systemctl start httpd

# install php 8 along with several necessary extensions for wordpress to run
sudo dnf install -y \
php \
php-cli \
php-cgi \
php-curl \
php-mbstring \
php-gd \
php-mysqlnd \
php-gettext \
php-json \
php-xml \
php-fpm \
php-intl \
php-zip \
php-bcmath \
php-ctype \
php-fileinfo \
php-openssl \
php-pdo \
php-tokenizer

# install the mysql version 8 community repository
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm 
#
# install the mysql server
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm 
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf repolist enabled | grep "mysql.*-community.*"
sudo dnf install -y mysql-community-server 
#
# start and enable the mysql server
sudo systemctl start mysqld
sudo systemctl enable mysqld

# environment variable
EFS_DNS_NAME=fs-09bcdf76765bbe6ad.efs.us-east-1.amazonaws.com

# mount the efs to the html directory 
echo "$EFS_DNS_NAME:/ /var/www/html nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 0 0" >> /etc/fstab
mount -a

# set permissions
chown apache:apache -R /var/www/html

# restart the webserver
sudo service httpd restart

## Conclusion
This project demonstrates a comprehensive deployment of a WordPress website on AWS infrastructure using DevOps practices. It ensures high availability, scalability, and fault tolerance, providing a robust solution for hosting WordPress applications.

Feel free to contribute, suggest improvements, or report issues.
