[Production-Grade 3‑Tier MERN Deployment on AWS with Docker, Nginx, CI/CD & Terraform](https://medium.com/@ravikantdiwakar/production-grade-3-tier-mern-deployment-on-aws-with-docker-nginx-ci-cd-terraform-42c081047e12)

# Book Store MERN Stack

A full-stack **MERN** (MongoDB, Express, React, Node.js) application for managing books with complete CRUD operations.

![MERN Stack](https://img.shields.io/badge/MERN-Stack-green)

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

## Project Structure

```
Book-Store-MERN-Stack/
├── backend/       # API, Routes, Models, Config
├── frontend/      # React App, Components, Pages
├── .gitignore
└── README.md
```

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
   PORT=5555
   MONGODB_URL=your_mongodb_connection_string
   ```
   > **Note:** replace `your_mongodb_connection_string` with your actual MongoDB URI.

4. **Run the server:**
   ```bash
   npm run dev
   ```
   The server should start on port `5555` (or your configured port).

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

## Troubleshooting

- **Database Connection Error**: If you see `ENOTFOUND` or connection timeout errors, ensure your `MONGODB_URL` in `.env` is correct and your IP address is whitelisted in MongoDB Atlas.
- **CORS Errors**: If the frontend cannot access the backend, ensure CORS is correctly configured in `backend/index.js` to allow the frontend's origin.
