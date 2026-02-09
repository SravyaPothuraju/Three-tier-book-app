

# Book Store App

A full-stack Book application for managing books with complete CRUD operations.

## Features

- **CRUD Operations**: Create, read, update, and delete books.
- **RESTful API**: Robust backend handling requests and responses.
- **Database**: Persistent storage using MongoDB and Mongoose.
- **Responsive Frontend**: Modern React UI with Tailwind CSS.
- **Components**: Reusable components, modals, and notifications.

## Tech Stack

- **Frontend**: React, Vite, React Router, Tailwind CSS
- **Backend**: Node.js, Express.js
- **Database**: MongoDB (Mongoose ODM)


## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v14 or higher)
- [MongoDB Atlas](https://www.mongodb.com/atlas/database) account or local MongoDB instance

### Backend Setup

1. **Navigate to the backend directory:**
   ```bash
   cd backend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure Environment Variables:**
   Create a `.env` file in the `backend` directory based on `.env.example`.
   ```bash
   cp .env.example .env
   ```
   
   Open `.env` and add your MongoDB connection string:
   ```env
   PORT=3000
   MONGODB_URL=your_mongodb_connection_string
   ```
   > **Note:** replace `your_mongodb_connection_string` with your actual MongoDB URI.

4. **Run the server:**
   ```bash
   npm run dev
   ```
   The server should start on port `3000` (or your configured port).

### Frontend Setup

1. **Open a new terminal and navigate to the frontend directory:**
   ```bash
   cd frontend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Run the application:**
   ```bash
   npm run dev
   ```
   Open your browser to the URL shown (usually `http://localhost:5173`).

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/books` | Get all books |
| GET | `/books/:id` | Get a specific book |
| POST | `/books` | Create a new book |
| PUT | `/books/:id` | Update a book |
| DELETE | `/books/:id` | Delete a book |

###Deployment on AWS 

🚀 Three-Tier Application Deployment on AWS

This guide explains how to deploy the Three-Tier Book App on AWS using a secure, highly available architecture with VPC, Application Load Balancer, private EC2 instances, NAT Gateway, IAM roles, and MongoDB Atlas.

🧱 Architecture Overview

Frontend: React + Vite (Port 5173)

Backend: Node.js + Express (Port 3000)

Database: MongoDB Atlas (managed external service)

Load Balancer: Internet-facing Application Load Balancer

Networking: Custom VPC with public and private subnets across 2 Availability Zones

Security: No public EC2, no SSH, access via AWS Systems Manager (SSM)

🏗️ High-Level Architecture Flow
User
 → Application Load Balancer (Public)
 → Web EC2 (React – 5173, Private)
 → App EC2 (Node – 3000, Private)
 → MongoDB Atlas (External Managed DB)

1️⃣ Create a Custom VPC

Open AWS VPC Console

Create a VPC

CIDR block: 10.0.0.0/16

Enable DNS hostnames

Enable DNS resolution

This VPC will host all AWS resources for the application.

2️⃣ Create Subnets (Multi-AZ)

Create subnets across two Availability Zones.

Public Subnets (for ALB)

Public Subnet AZ-1 → 10.0.1.0/24

Public Subnet AZ-2 → 10.0.2.0/24

Private Web Subnets (Frontend)

Web Subnet AZ-1 → 10.0.11.0/24

Web Subnet AZ-2 → 10.0.12.0/24

Private App Subnets (Backend)

App Subnet AZ-1 → 10.0.21.0/24

App Subnet AZ-2 → 10.0.22.0/24

⚠️ There are no database subnets because MongoDB Atlas is external.

3️⃣ Internet Gateway (IGW)

Create an Internet Gateway

Attach it to the VPC

The IGW allows internet access only for public resources such as the ALB.

4️⃣ NAT Gateway

Allocate an Elastic IP

Create a NAT Gateway in Public Subnet AZ-1

Attach the Elastic IP

The NAT Gateway allows private EC2 instances to access the internet outbound only (npm install, MongoDB Atlas, updates).

5️⃣ Route Tables
Public Route Table

Associate with:

Public Subnet AZ-1

Public Subnet AZ-2

Route:

0.0.0.0/0 → Internet Gateway

Private Route Table

Associate with:

Web Subnets

App Subnets

Route:

0.0.0.0/0 → NAT Gateway

6️⃣ Security Groups
ALB Security Group

Inbound

HTTP (80) from 0.0.0.0/0

Outbound

Port 5173 → Web EC2 Security Group

Web EC2 Security Group

Inbound

Port 5173 only from ALB Security Group

Outbound

Port 3000 → App EC2 Security Group

HTTPS (443) → Internet (via NAT)

App EC2 Security Group

Inbound

Port 3000 only from Web EC2 Security Group

Outbound

HTTPS (443) → 0.0.0.0/0 (MongoDB Atlas)

7️⃣ IAM Role for EC2 (No SSH)

Create an IAM role:

Service: EC2

Policy attached:

AmazonSSMManagedInstanceCore

Attach this role to all EC2 instances.

✅ Enables AWS Systems Manager Session Manager
❌ No SSH keys or port 22 required

8️⃣ Launch EC2 Instances
Web Tier EC2 (Frontend)

Instances: 2 (one per AZ)

Subnet: Private Web Subnets

Public IP: ❌ No

Security Group: Web EC2 SG

IAM Role: SSM Role

AMI: Amazon Linux 2

Instance type: t3.micro

User Data

#!/bin/bash
yum update -y
curl -fsSL https://rpm.nodesource.com/setup_18.x | bash -
yum install -y nodejs git

cd /home/ec2-user
git clone https://github.com/SravyaPothuraju/Three-tier-book-app.git
cd Three-tier-book-app/frontend

npm install
npm run dev -- --host

App Tier EC2 (Backend)

Instances: 2 (one per AZ)

Subnet: Private App Subnets

Public IP: ❌ No

Security Group: App EC2 SG

IAM Role: SSM Role

User Data

#!/bin/bash
yum update -y
curl -fsSL https://rpm.nodesource.com/setup_18.x | bash -
yum install -y nodejs git

cd /home/ec2-user
git clone https://github.com/SravyaPothuraju/Three-tier-book-app.git
cd Three-tier-book-app/backend

npm install
node index.js

9️⃣ Application Load Balancer (ALB)

Create an Application Load Balancer

Internet-facing

Select both public subnets

Attach ALB Security Group

Target Group

Target type: Instance

Protocol: HTTP

Port: 5173

Health check path: /

ALB forwards traffic only to Web EC2 instances.

🔟 MongoDB Atlas Configuration

Create a MongoDB Atlas cluster

Network Access:

Allow 0.0.0.0/0 (demo) or NAT Elastic IP

Create database user

Backend Environment Variables
MONGO_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/bookdb
PORT=3000


The backend connects securely over HTTPS (443).

1️⃣1️⃣ Accessing EC2 Instances (SSM)

Open AWS Systems Manager

Go to Session Manager

Start a session with Web or App EC2

No SSH, no key pairs, no open ports.

🔒 Security & Best Practices

No public EC2 instances

No SSH access

Strict security group rules

Private subnets for all compute

Managed database (MongoDB Atlas)

High availability across two AZs

🎉 Congratulations!

You’ve successfully designed and documented a secure, highly available three-tier MERN application on AWS using industry best practices. This project demonstrates real-world cloud architecture skills, including network isolation with VPCs and subnets, secure access using IAM and AWS Systems Manager, traffic management with an Application Load Balancer, and modern database integration with MongoDB Atlas.

Kudos for building this the right way — no public EC2s, no SSH, and a clean separation of tiers across multiple Availability Zones. This architecture is interview-ready and resume-worthy.


✨ Tada! You’re officially cloud-architect material. Keep building, keep breaking, and keep learning! 🚀