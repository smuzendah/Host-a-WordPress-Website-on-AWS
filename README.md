```markdown

![alt text](Host_a_WordPress_Website_on_AWS.png)

# AWS WordPress Deployment Project

## Overview
This project focuses on deploying a WordPress website on Amazon Web Services (AWS) infrastructure utilizing various AWS services and resources to ensure high availability, scalability, and fault tolerance.

## Project Components
1. **Virtual Private Cloud (VPC)**:
   - Configured a VPC with both public and private subnets across two different availability zones.
2. **Internet Gateway**:
   - Deployed an Internet Gateway to facilitate connectivity between VPC instances and the wider Internet.
3. **Security Groups**:
   - Established Security Groups as a network firewall mechanism.
4. **Availability Zones**:
   - Leveraged two Availability Zones to enhance system reliability and fault tolerance.
5. **Public Subnets**:
   - Utilized Public Subnets for infrastructure components like the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint**:
   - Implemented EC2 Instance Connect Endpoint for secure connections to assets within both public and private subnets.
7. **Web Servers (EC2 instances)**:
   - Positioned web servers within Private Subnets for enhanced security.
8. **NAT Gateway**:
   - Enabled instances in both the private Application and Data subnets to access the Internet via the NAT Gateway.
9. **WordPress Hosting**:
   - Hosted the website on EC2 Instances.
10. **Application Load Balancer (ALB)**:
    - Employed an Application Load Balancer and a target group for evenly distributing web traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.
11. **Auto Scaling Group**:
    - Utilized an Auto Scaling Group to automatically manage EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.
12. **Version Control**:
    - Stored web files on GitHub for version control and collaboration.
13. **Certificate Manager**:
    - Secured application communications between the users and the websites using a Certificate Manager.
14. **Simple Notification Service (SNS)**:
    - Configured SNS to alert about activities within the Auto Scaling Group.
15. **Domain Registration**:
    - Registered the domain name and set up a DNS record using Route 53.
16. **Shared File System**:
    - Used Amazon EFS for a shared file system.
17. **Database**:
    - Employed Amazon RDS for the database.

## Deployment Scripts
### Script to Install WordPress
```bash
# Script to install WordPress...
```

### Script for Auto Scaling Group Launch Template
#!/bin/bash

#Script to install WordPress

# create to root user
sudo su
# update the software packages on the ec2 instance
sudo yum update -y
# create an html directory
sudo mkdir -p /var/www/html
# environment variable
EFS_DNS_NAME=fs-064e9505819af10a4.efs.us-east-1.amazonaws.com
# mount the efs to the html directory
sudo mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport
"$EFS_DNS_NAME":/ /var/www/html
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
# set permissions
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
sudo find /var/www -type f -exec sudo chmod 0664 {} \;
chown apache:apache -R /var/www/html
# download wordpress files
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/
# create the wp-config.php file
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
# edit the wp-config.php file
sudo vi /var/www/html/wp-config.php
# restart the webserver
sudo service httpd restart
Script for Auto scaling group launch template
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
EFS_DNS_NAME=fs-02d3268559aa2a318.efs.us-east-1.amazonaws.com
# mount the efs to the html directory
echo "$EFS_DNS_NAME:/ /var/www/html nfs4
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 0 0" >> /etc/fstab
mount -a
# set permissions
chown apache:apache -R /var/www/html
# restart the webserver
sudo service httpd restart


# Script for Auto Scaling Group launch template...
```

## Lessons Learned
- Utilized AWS documentation and Stack Overflow during configuration troubleshooting.

## Contributors
- [Sylvester Muzendah]

## References
- [AWS Documentation](https://aws.amazon.com/documentation/)
- [Stack Overflow](https://stackoverflow.com/)
```

Feel free to customize this README file according to your project's specific details and preferences.
