# Three-Tier-Architecture-for-Web-Application-on-AWS
You have a web application that accepts requests from the internet. Clients can send requests to query for data. When a request comes in, the web application queries a MySQL database and returns the data to the client.


# Document Purpose

This repository documents the design and implementation of a three-tier web application architecture using AWS services. The architecture ensures high availability, scalability, and security for a web application that accepts internet requests, queries a MySQL database, and returns data to clients.

## Architecture Overview

The clients can connect to the web application through the Internet Gateway attached to the VPC. The requests are received by the ELB that distributes traffic to EC2 Instances hosted on different Availability Zones. The web facing EC2 instances are under Public Subnet.

The Auto Scaling Groups attached to the instances manages the scaling based on the rise and fall of internet traffic. The security of the Public Subnet is controlled by the Security Group attached to it. This security group allows traffic only from the Internet Gateway.

The EC2 instances in turn communicates with the MySQL Databases hosted on different Availability Zones and shares the response back to the client through the same channel.

The MySQL databases are under a Private Subnet away from direct internet access.

The Auto Scaling Groups attached to the database manages the scaling based on the rise and fall of EC2 traffic.

The security of the Private Subnet is controlled by the Security Group attached to it. This security group allows traffic only from the security group of the EC2 instances.

The instances are placed under 2 Availability Zones configured with in the VPC for high availability and the VPS is under single Region.

The Internet Gateway and Application Load Balancer is scalable and highly available by design. Hence, they can automatically scale base on the web traffic coming in to the solution.
### Components

1. **Networking Layer**
   - **Amazon VPC**: A virtual private cloud to host our infrastructure.
   - **Subnets**: Public and private subnets to segregate resources.
  
2. **Compute Layer**
   - **Elastic Load Balancer (ELB)**: Distributes incoming traffic across EC2 instances.
   - **Amazon EC2**: Instances running the web application, auto-scaled for high availability.

3. **Database Layer**
   - **Amazon RDS (MySQL)**: Managed relational database service with Multi-AZ deployment for high availability.

### Traffic Flow

1. **Client Requests**: Sent to the ELB.
2. **Load Balancer**: Distributes requests to EC2 instances.
3. **Application Servers**: Process requests and query the RDS database if needed.
4. **Database**: Processes queries and returns data to the application servers.
5. **Response**: Application servers send the response back to the client via the ELB.

### AWS Services Justification

- **Amazon VPC**: Provides network isolation and security.
- **Elastic Load Balancing (ELB)**: Ensures high availability and distributes traffic.
- **Amazon EC2**: Scalable compute resources with auto-scaling.
- **Amazon RDS (MySQL)**: Managed database with automated backups, patching, and high availability.

### Getting Started

1. **VPC Setup**: Create a VPC with public and private subnets.
2. **ELB Setup**: Configure an Elastic Load Balancer in the public subnet.
3. **EC2 Setup**: Launch EC2 instances in private subnets and configure Auto Scaling.
4. **RDS Setup**: Launch an RDS instance with Multi-AZ deployment in private subnets.
5. **Security Groups**: Set up security groups to allow necessary traffic between components.

### Contact

For any questions or support, please contact Mohamed Ashraf at eng.mohamedashrafm@gmail.com

