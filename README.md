Note: The base application used in this project was adapted from an existing source.  
My primary focus in this repository is on **Cloud, AWS architecture, and infrastructure setup** rather than application development.

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


📚 Three-Tier Book App – AWS Deployment

This project demonstrates a production-style three-tier MERN application deployed on AWS using best practices for network isolation, security, and high availability. The application is hosted entirely inside a custom VPC and exposed to the internet only through an Application Load Balancer (ALB).

🧩 Project Overview

Frontend: React (Vite) – Web Tier

Backend: Node.js + Express – Application Tier

Database: MongoDB Atlas (Managed External Service)

Cloud Provider: AWS

         Architecture Type: Three-Tier Architecture

         Availability: Multi-AZ (High Availability)

         Security: No public EC2, no SSH, IAM + AWS Systems Manager

🏗️ Architecture Overview

The application is deployed inside a custom Amazon VPC spanning two Availability Zones. The architecture is logically divided into three tiers:

1️⃣ Web Tier (Frontend)

         React application

         Runs on EC2 instances in private subnets

         Receives traffic only from the Application Load Balancer

2️⃣ Application Tier (Backend)

         Node.js + Express API

         Runs on EC2 instances in private subnets

         Receives traffic only from the Web Tier

3️⃣ Data Tier

         MongoDB Atlas (managed external database)

         Accessed securely over outbound HTTPS

         No database hosted on EC2

🌐 High-Level Architecture Flow
User
 → Application Load Balancer (Public Subnets)
 → Web EC2 (React – Private Subnets)
 → App EC2 (Node.js – Private Subnets)
 → MongoDB Atlas (External Managed Service)

🔐 Key AWS Components Used

         VPC – Network isolation

         Subnets – Tier-based segmentation

         Internet Gateway (IGW) – Public access for ALB

         NAT Gateway – Outbound internet access for private EC2

         Route Tables – Traffic control

         Security Groups – Least-privilege networking

         Application Load Balancer – Traffic distribution

         IAM Role (AmazonSSMManagedInstanceCore) – Secure EC2 access

         AWS Systems Manager (SSM) – No SSH access

🛡️ Security Design Highlights

         No EC2 instance has a public IP

         No SSH or port 22 access

         All EC2 access via AWS Systems Manager

         Strict Security Group rules between tiers

         Database managed externally (MongoDB Atlas)

⚙️ How the Application Works

         A user accesses the application using the ALB DNS name

         The ALB forwards requests to the Web Tier EC2 instances

         The React frontend sends API requests to the Backend EC2

         The backend processes requests and connects to MongoDB Atlas

Responses flow back through the same path

🚀 Deployment Steps (Step-by-Step)
Step 1: Create a Custom VPC

         CIDR: 10.0.0.0/16

         Enable DNS hostnames and resolution

Step 2: Create Subnets (2 AZs)

         Public Subnets (ALB):

         10.0.1.0/24

         10.0.2.0/24

         Private Web Subnets (Frontend):

         10.0.11.0/24

         10.0.12.0/24

         Private App Subnets (Backend):

         10.0.21.0/24

         10.0.22.0/24

Step 3: Create and Attach Internet Gateway

         Attach IGW to the VPC

         Used only by public subnets

Step 4: Create NAT Gateway

         Create in a public subnet

         Attach Elastic IP

         Enables outbound internet access for private EC2

         Step 5: Configure Route Tables

         Public Route Table

         0.0.0.0/0 → Internet Gateway


         Private Route Table

         0.0.0.0/0 → NAT Gateway

Step 6: Create Security Groups

         ALB SG

         Inbound: HTTP (80) from anywhere

         Outbound: Port 5173 to Web SG

         Web EC2 SG

         Inbound: 5173 from ALB SG

         Outbound: 3000 to App SG, 443 to internet

         App EC2 SG

         Inbound: 3000 from Web SG

         Outbound: 443 to MongoDB Atlas

Step 7: Create IAM Role for EC2

         Role: EC2

         Policy: AmazonSSMManagedInstanceCore

         Attach to all EC2 instances

Step 8: Launch EC2 Instances

         Web Tier EC2

         Private Web Subnets

         No public IP

         React app served as production build

         App Tier EC2

         Private App Subnets

         No public IP

         Node.js backend running on port 3000

Step 9: Configure Application Load Balancer

         Internet-facing

         Public subnets

         Target Group:

         Type: Instance

         Port: 80 or 3000 (depending on frontend server)

         Health check path: /

Step 10: Configure MongoDB Atlas

         Create cluster

         Allow network access from NAT Gateway IP or 0.0.0.0/0

Set backend environment variables:

         MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/bookdb
         PORT=3000

Step 11: Access EC2 via SSM

AWS Console → Systems Manager → Session Manager

Start session (no SSH keys required)

🧪 How to Verify Deployment

Check Target Group → Healthy

         Open browser:

         http://<ALB-DNS-NAME>


Application should load successfully


“This project implements a secure, highly available three-tier MERN architecture on AWS using a public Application Load Balancer, private EC2 instances managed through IAM and AWS Systems Manager, and MongoDB Atlas as a managed database service.”

🎉 Final Note

✨ Tada!
You’ve successfully built and deployed a real-world, production-aligned AWS architecture with proper security, scalability, and clean tier separation.
Kudos for doing it the right way — this project is resume-ready and interview-ready. 🚀