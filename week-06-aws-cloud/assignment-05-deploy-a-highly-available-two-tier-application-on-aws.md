# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS (VPC + ALB + ASG + Multi-AZ RDS)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![alt text](screenshots/Assignment-05-Task-01-screenshot-01.png)

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

![alt text](screenshots/Assignment-05-Task-01-screenshot-02.png)

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

![alt text](screenshots/Assignment-05-Task-01-screenshot-03a.png)
![alt text](screenshots/Assignment-05-Task-01-screenshot-03b.png)

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

![alt text](screenshots/Assignment-05-Task-01-screenshot-04a.png)
![alt text](screenshots/Assignment-05-Task-01-screenshot-04b.png)
---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

![alt text](screenshots/Assignment-05-Task-01-screenshot-05.png)

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

![alt text](screenshots/Assignment-05-Task-02-screenshot-06.png)

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

![alt text](screenshots/Assignment-05-Task-02-screenshot-07.png)

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

![alt text](screenshots/Assignment-05-Task-02-screenshot-08.png)

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

![alt text](screenshots/Assignment-05-Task-03-screenshot-09.png)

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

![alt text](screenshots/Assignment-05-Task-03-screenshot-10.png)

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

![alt text](screenshots/Assignment-05-Task-04-screenshot-11.png)

---

#### Screenshot 12 — A running instance created from the template showing that the application responds on port 80 through a local test or browser using its public IP

![alt text](screenshots/Assignment-05-Task-04-screenshot-12.png)

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

![alt text](screenshots/Assignment-05-Task-05-screenshot-13.png)

---

#### Screenshot 14 — Target group showing at least one healthy target

![alt text](screenshots/Assignment-05-Task-05-screenshot-14.png)

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

![alt text](screenshots/Assignment-05-Task-06-screenshot-15.png)

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

![alt text](screenshots/Assignment-05-Task-06-screenshot-16.png)

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

![alt text](screenshots/Assignment-05-Task-07-screenshot-17.png)

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

![alt text](screenshots/Assignment-05-Task-07-screenshot-18.png)

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB.

Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance; timestamps are helpful

![alt text](screenshots/Assignment-05-Task-08-screenshot-19a.png)
![alt text](screenshots/Assignment-05-Task-08-screenshot-19b.png)

---

#### Screenshot 20 — Target group showing healthy targets after replacement

![alt text](screenshots/Assignment-05-Task-08-screenshot-20.png)

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

![alt text](screenshots/Assignment-05-Task-08-screenshot-21.png)

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

![alt text](screenshots/Assignment-05-Task-08-screenshot-22.png)

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — A simple architecture diagram, which may be hand-drawn, or an AWS console overview showing the components

![alt text](screenshots/Assignment-05-Task-09-screenshot-23.png)

---

### Notes

Summarize the VPC and subnets across the two Availability Zones.

VPC and Subnets:
Created a VPC (10.0.0.0/16) spanning two Availability Zones, with two public subnets (10.0.1.0/24 in AZ-a, 10.0.2.0/24 in AZ-b) for the web tier and ALB, and two private subnets (10.0.11.0/24, 10.0.12.0/24) for the database tier. An Internet Gateway routes public subnet traffic, while a NAT Gateway with an Elastic IP allows private subnets outbound internet access without direct exposure.

Summarize the ALB and Auto Scaling Group setup.

ALB and Auto Scaling Group:
Deployed an internet-facing Application Load Balancer across both public subnets, forwarding HTTP traffic to a target group. An Auto Scaling Group (desired: 2, min: 2, max: 4) launches WordPress instances from a self-configuring Launch Template across both Availability Zones, registering automatically with the ALB's target group

Summarize the private Multi-AZ RDS setup.

rivate Multi-AZ RDS:
Deployed a MySQL RDS instance with Multi-AZ enabled, placed in the private DB subnet group, with no public access. The database is only reachable from the web tier via a scoped Security Group rule (ha-db-sg accepting traffic solely from ha-web-sg).

Summarize the results of both high-availability tests.

High-Availability Test Results:
Test A confirmed that terminating a web instance triggered automatic replacement by the Auto Scaling Group, with the ALB continuing to serve traffic throughout, and the target group returning to a fully healthy state once the new instance registered. Test B confirmed that stopping an instance in one Availability Zone had no impact on application availability — the ALB continued routing all traffic successfully through the remaining healthy instance in the other AZ.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://www.linkedin.com/posts/emmanuel-sunday-210a08323_dmibypravinmishra-agenticai-devops-activity-7493767442117345280-Hf9F?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFHXXywBq0IrgBBhbi5ULmCrDuZgCEYc6fQ

---

#### Screenshot of LinkedIn post

![alt text](screenshots/Assignment-05-Task-10-screenshot-24.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [ ] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [ ] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [ ] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [ ] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [ ] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [ ] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [ ] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [ ] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [ ] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [ ] LinkedIn post published and URL submitted
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*