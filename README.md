# DevOps Project: Hosting WordPress on AWS with High Availability and Scalability

This project focuses on deploying a WordPress website on Amazon Web Services (AWS) infrastructure using various services and resources to ensure high availability, scalability, and fault tolerance. Below is a detailed guide on how to set up and deploy the WordPress website on AWS.

## Architecture Overview
The architecture consists of several AWS services:

- **Virtual Private Cloud (VPC)**: Configured with public and private subnets across two availability zones for improved fault tolerance.
- **Internet Gateway**: Facilitates connectivity between VPC instances and the wider Internet.
- **Security Groups**: Acts as a network firewall mechanism to control inbound and outbound traffic.
- **EC2 Instances**: Web servers (WordPress) hosted within private subnets for enhanced security.
- **NAT Gateway**: Allows instances in private subnets to access the Internet.
- **Application Load Balancer (ALB)**: Distributes web traffic evenly to an Auto Scaling Group of EC2 instances across multiple availability zones.
- **Auto Scaling Group**: Automatically manages EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.
- **Amazon EFS**: Shared file system used for storing web files for multiple EC2 instances.
- **Amazon RDS**: Managed relational database service used for hosting the WordPress database.
- **Route 53**: Registers the domain name and sets up DNS records.
- **Certificate Manager**: Provides SSL/TLS certificates to secure application communications.
- **Simple Notification Service (SNS)**: Alerts about activities within the Auto Scaling Group.

## Setup Instructions
Follow these steps to deploy the WordPress website on AWS:

1. **Clone the Repository**: Clone the GitHub repository containing the reference diagram and deployment scripts.

2. **Configuration of Infrastructure**:
   - Set up a VPC with public and private subnets across two availability zones.
   - Deploy an Internet Gateway for Internet connectivity.
   - Configure Security Groups for network security.
   - Utilize NAT Gateway for instances in private subnets to access the Internet.
   - Implement EC2 Instance Connect Endpoint for secure connections.
   - Configure Route 53 for domain registration and DNS record setup.
   - Utilize EFS for shared file storage and RDS for the database.

3. **Installation of WordPress**:
   - Execute the provided script to install WordPress on EC2 instances.
   - Update software packages.
   - Install and configure Apache web server with PHP and necessary extensions.
   - Install MySQL server and set up permissions.
   - Mount EFS to store web files.
   - Set permissions and restart the webserver.

4. **Auto Scaling Group Configuration**:
   - Use the provided script to set up an Auto Scaling Group launch template.
   - Update software packages.
   - Install and configure Apache web server with PHP and necessary extensions.
   - Install MySQL server and set up permissions.
   - Mount EFS and configure permissions.
   - Restart the webserver.

5. **Lessons Learned**:
   - Document any lessons learned during the project, including references to AWS documentation and resources used for troubleshooting.

## Contributors and References
- Mention any contributors to the project.
- Acknowledge any external resources or documentation used during the project.

## Conclusion
This README provides a comprehensive guide for deploying a WordPress website on AWS infrastructure using DevOps practices. Following these instructions will ensure a highly available, scalable, and fault-tolerant web application setup.

For further details, refer to the repository's documentation and scripts.

[Link to GitHub Repository](#) (https://github.com/smuzendah/Host-a-WordPress-Website-on-AWS/tree/main)

Feel free to contribute, suggest improvements, or report issues.

---
This README template is intended to guide you in creating documentation for your AWS WordPress deployment project. Modify it as per your project's specific requirements and details.
