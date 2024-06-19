# AWS Web Application Architecture Setup

This guide details the steps to set up a highly available and scalable web application architecture on AWS, which includes a VPC, subnets, Internet Gateway, security groups, EC2 instances, RDS MySQL databases, and an Application Load Balancer.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Step 1: Set Up VPC and Subnets](#step-1-set-up-vpc-and-subnets)
- [Step 2: Set Up the Internet Gateway](#step-2-set-up-the-internet-gateway)
- [Step 3: Configure Route Tables](#step-3-configure-route-tables)
- [Step 4: Set Up Security Groups](#step-4-set-up-security-groups)
- [Step 5: Launch EC2 Instances (Web Servers)](#step-5-launch-ec2-instances-web-servers)
- [Step 6: Launch MySQL Instances (Databases)](#step-6-launch-mysql-instances-databases)
- [Step 7: Configure Auto Scaling for RDS](#step-7-configure-auto-scaling-for-rds)
- [Step 8: DNS Configuration (Optional)](#step-8-dns-configuration-optional)
- [Step 9: Testing and Monitoring](#step-9-testing-and-monitoring)

## Prerequisites

- AWS Account
- Basic knowledge of AWS services

## Step 1: Set Up VPC and Subnets

1. **Create a VPC:**
   - Navigate to the VPC Dashboard.
   - Click on "Create VPC".
   - Provide a name, IPv4 CIDR block (e.g., `10.0.0.0/16`).
   - Click "Create VPC".

2. **Create Subnets:**
   - Go to the Subnets section within the VPC Dashboard.
   - Click "Create subnet".
   - Create two public subnets in different Availability Zones (e.g., `10.0.1.0/24`, `10.0.2.0/24`).
   - Create two private subnets in different Availability Zones (e.g., `10.0.3.0/24`, `10.0.4.0/24`).

## Step 2: Set Up the Internet Gateway

1. **Create an Internet Gateway:**
   - Navigate to the Internet Gateways section in the VPC Dashboard.
   - Click "Create internet gateway".
   - Attach the Internet Gateway to your VPC.

## Step 3: Configure Route Tables

1. **Create and Configure Route Tables:**
   - Go to Route Tables in the VPC Dashboard.
   - Create a route table for the public subnets.
   - Edit routes to add a route for `0.0.0.0/0` pointing to the Internet Gateway.
   - Associate this route table with the public subnets.
   - Create another route table for the private subnets.

## Step 4: Set Up Security Groups

1. **Create Security Groups:**
   - Go to the Security Groups section.
   - Create a security group for the public subnets (web servers).
     - Allow inbound HTTP (port 80) and HTTPS (port 443) traffic from anywhere (`0.0.0.0/0`).
     - Allow outbound traffic to the private subnets and the Internet.
   - Create a security group for the private subnets (databases).
     - Allow inbound MySQL traffic (port 3306) from the security group of the web servers.
     - Allow outbound traffic to the web servers.

## Step 5: Launch EC2 Instances (Web Servers)

1. **Create an Application Load Balancer (ALB):**
   - Navigate to the EC2 Dashboard, then to Load Balancers.
   - Click "Create Load Balancer" and choose Application Load Balancer.
   - Configure the ALB to have listeners on ports 80 and 443.
   - Add the public subnets to the ALB.
   - Create a target group and register your EC2 instances (web servers).

2. **Create an Auto Scaling Group for Web Servers:**
   - Navigate to Auto Scaling Groups.
   - Click "Create Auto Scaling group".
   - Attach the launch configuration (or launch template) for the EC2 instances.
   - Configure the Auto Scaling group to span the public subnets.
   - Attach the target group of the ALB to the Auto Scaling group.

## Step 6: Launch MySQL Instances (Databases)

1. **Create an RDS MySQL Instance:**
   - Navigate to the RDS Dashboard.
   - Click "Create database".
   - Choose MySQL as the database engine.
   - Configure the RDS instance to use the private subnets.
   - Ensure the security group for the database allows traffic only from the web servers' security group.

## Step 7: Configure Auto Scaling for RDS

1. **Enable Auto Scaling for RDS:**
   - Navigate to the RDS instance.
   - Modify the instance to enable Auto Scaling if needed (this can also be managed through Aurora for MySQL).

## Step 8: DNS Configuration (Optional)

1. **Set Up Route 53:**
   - Navigate to the Route 53 Dashboard.
   - Create a hosted zone.
   - Create DNS records to point to the Application Load Balancer.

## Step 9: Testing and Monitoring

1. **Test the Setup:**
   - Access the web application using the ALB DNS name.
   - Ensure the application can query data from the MySQL database and return it to the client.

2. **Set Up Monitoring and Alerts:**
   - Use CloudWatch to monitor the health of EC2 instances and RDS instances.
   - Set up necessary alarms for CPU usage, memory, and other critical metrics.

## Conclusion

By following these steps, you will have a robust, scalable, and highly available web application architecture on AWS. This setup ensures secure communication between web servers and databases, leveraging multiple Availability Zones, an Internet Gateway, ALB, and Auto Scaling Groups.
