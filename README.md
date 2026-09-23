# Assignment-01: Load Balancer & Auto Scaling Group

## Objective

The objective of this assignment is to design and implement a highly available and scalable AWS infrastructure for the Spring3Hibernate application.

The application is deployed on private EC2 instances and accessed through an Internet-facing Application Load Balancer.

## Application

Spring3Hibernate:

https://github.com/opstree/spring3hibernate.git

## AWS Architecture

The infrastructure consists of:

* Amazon VPC
* Internet Gateway
* Public Subnets
* Private Subnets
* NAT Gateway
* Application Load Balancer
* Target Group
* Auto Scaling Group
* Private EC2 instances
* Security Groups


  VPC-Screenshots#################


  <img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 01 37 AM" src="https://github.com/user-attachments/assets/c7e493d4-e6f6-4da5-a430-7899ff03da7e" />


####Subnets-Screenshots#################


<img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 03 58 AM" src="https://github.com/user-attachments/assets/ad1cd1f7-d3a0-4051-8bb6-e8cd92ff37f3" />


## Network Configuration

| Resource         | Configuration                |
| ---------------- | ---------------------------- |
| VPC              | `10.0.0.0/16`                |
| Public Subnet 1  | `10.0.1.0/24` - ap-south-1a  |
| Public Subnet 2  | `10.0.2.0/24` - ap-south-1b  |
| Private Subnet 1 | `10.0.11.0/24` - ap-south-1a |
| Private Subnet 2 | `10.0.12.0/24` - ap-south-1b |
| Region           | ap-south-1                   |

## Load Balancer

* Type: Application Load Balancer
* Name: `spring3hibernate-alb`
* Scheme: Internet-facing
* Listener: HTTP 80
* Target Group: `spring3hibernate-tg`
* Target Port: 8080

## Load Balancer-Screenshots######


<img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 06 52 AM" src="https://github.com/user-attachments/assets/c7b07f20-47ab-4b26-9c82-a9d535eb4972" />



## Auto Scaling Group

* Name: `spring3hibernate-asg`
* Minimum Capacity: 2
* Desired Capacity: 2
* Maximum Capacity: 4
* Availability Zones:

  * ap-south-1a
  * ap-south-1b
* Health Checks: EC2 + ELB
* Target Tracking: Average CPU Utilization 50%

  #######Auto Scaling Group-Screenshots######



  <img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 06 52 AM" src="https://github.com/user-attachments/assets/f8172e8c-6798-476b-8b87-9d1c5c1cbc31" />


## EC2 Application Servers

The application servers are deployed in private subnets.

* Instance Type: `t3.micro`
* Application Port: `8080`
* Application Server: Tomcat 7
* Java: Java 8
* Application: Spring3Hibernate

The EC2 instances do not have public IP addresses.

## EC2 Application Servers-Screenshots######

<img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 10 07 AM" src="https://github.com/user-attachments/assets/69c4f636-f75e-4a88-a3bb-caf4f9ceb735" />



## NAT Gateway

The NAT Gateway provides outbound internet connectivity to the private application servers.

Private subnet route:

```text
0.0.0.0/0 → NAT Gateway
```
<img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 12 26 AM" src="https://github.com/user-attachments/assets/c85891c7-8101-4386-a1e4-a797a2f59db8" />


## Security

### ALB Security Group

```text
Inbound:
HTTP 80 → 0.0.0.0/0
```
<img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 13 48 AM" src="https://github.com/user-attachments/assets/91e83163-070a-468a-820a-5c0fd61e1834" />


### Application Security Group

```text
Inbound:
TCP 8080 → ALB Security Group
SSH 22 → Required administrative access
```

The application servers are not directly exposed to the internet.

## Target Group Health

The Target Group uses:

```text
Protocol: HTTP
Port: 8080
Health Check Path: /
```

<img width="1440" height="900" alt="Screenshot 2026-09-23 at 11 17 30 AM" src="https://github.com/user-attachments/assets/321a5e6b-fa53-4707-b8ee-617167f73b45" />


Final verification:

```text
Healthy Targets: 2
Unhealthy Targets: 0
```

## Application Verification

The application was successfully accessed through the Application Load Balancer.

ALB DNS:

```text
http://spring3hibernate-alb-1305262024.ap-south-1.elb.amazonaws.com/
```

Application response:

```text
Sample WebApp CRUD Example for CI
```

Available application functions:

1. List of Employees
2. Add Employee
3. Upload File
4. List Images

## High Availability

The application servers are distributed across two Availability Zones:

```text
ap-south-1a → Private EC2
ap-south-1b → Private EC2
```

The Application Load Balancer distributes incoming requests between healthy targets.

The Auto Scaling Group maintains a minimum of two application instances and can scale up to four instances based on CPU utilization.

## Architecture Diagram

Add the final AWS architecture diagram here.

## Screenshots

### 1. VPC

Add VPC screenshot here.

### 2. Subnets

Add subnet configuration screenshot here.

### 3. Route Tables

Add public and private route table screenshots here.

### 4. NAT Gateway

Add NAT Gateway screenshot here.

### 5. Security Groups

Add Security Group screenshots here.

### 6. Application Load Balancer

Add ALB screenshot here.

### 7. Target Group

Add Target Group screenshot showing 2 healthy targets here.

### 8. Auto Scaling Group

Add ASG configuration screenshot here.

### 9. Launch Template

Add Launch Template Version 2 screenshot here.

### 10. Final Application

Add browser screenshot showing the Spring3Hibernate application here.

## Final Result

The Spring3Hibernate application was successfully deployed on private EC2 instances and exposed through an Application Load Balancer.

The final infrastructure provides:

* High availability across two Availability Zones
* Load balancing
* Auto scaling
* Private application servers
* NAT-based outbound internet access
* Security Group based traffic control
* Health checks for application instances

