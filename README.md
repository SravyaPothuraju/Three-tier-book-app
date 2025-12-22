# Book Store MERN Stack

A full-stack **MERN** (MongoDB, Express, React, Node.js) application for managing books with complete CRUD operations, built as part of a step‑by‑step tutorial series.[1]

The project is split into a backend REST API (Node.js/Express/MongoDB) and a frontend single‑page application (React with Vite and Tailwind CSS).[1]

***

## Features

- Create, read, update, and delete books with title, author, price, and other metadata using a RESTful API.[1]
- Persistent storage using MongoDB through Mongoose models and schemas.[1]
- Modern React frontend with React Router, SPA navigation, and reusable components such as a book card and modals.[1]
- Tailwind CSS styling with responsive layout and improved UX using alert/notification components.[1]
- Clean project structure with separate `backend` and `frontend` folders to mirror production‑grade MERN apps.[1]

***

## Tech Stack

- **Frontend**: React, Vite, React Router, Tailwind CSS.[1]
- **Backend**: Node.js, Express.js, Mongoose.[1]
- **Database**: MongoDB (local or Atlas).[1]
- **Other**: REST API, CORS, environment variables for configuration.[1]

***

## Project Structure

```bash
Book-Store-MERN-Stack/
├── backend/       # Node.js + Express + MongoDB API
├── frontend/      # React (Vite) client with Tailwind
├── .gitignore
└── README.md
```

- `backend`: Contains Express app, routes, controllers, and the Book Mongoose model.[1]
- `frontend`: Contains React pages, components, routing setup, and Tailwind configuration. A comment in `main.jsx` explains the root render and router wrapper.[1]


## Getting Started

### Prerequisites

- Node.js and npm installed on your machine.  
- MongoDB instance (local or MongoDB Atlas) with a connection URI.  

### Backend setup

```bash
cd backend
npm install
```

Create a `.env` file in `backend`:

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string
```

Then run the backend:

```bash
npm run dev   # or: npm start
```

The API will run on the configured port (commonly `http://localhost:5000`).  

### 3. Frontend setup

Open a new terminal from the project root and run:

```bash
cd frontend
npm install
npm run dev
```

The frontend will be available on the Vite dev server (commonly `http://localhost:5173` or similar).  

***

## API Overview

The backend exposes RESTful endpoints for managing books.[1]

Typical routes (prefix may vary, e.g. `/books`):

- `GET /books` – Get all books.  
- `GET /books/:id` – Get a single book by id.  
- `POST /books` – Create a new book.  
- `PUT /books/:id` – Update an existing book.  
- `DELETE /books/:id` – Delete a book.  

Request and response formats follow standard JSON conventions using Mongoose models for validation and database operations.[1]

***

## Frontend Overview

The React app is a SPA that communicates with the backend API.[1]

Key behaviors:

- Book list page that fetches and displays books, either as a table or card grid.[1]
- Detail page that shows information for a single book.[1]
- Forms and modals for creating and editing books with validation and feedback.[1]
- Delete actions with confirmation and alert messages to enhance UX.[1]

***

## Scripts

From `backend`:

- `npm start` – Run the production server.  
- `npm run dev` – Run the backend in development mode with live reload (e.g. using nodemon).  

From `frontend`:

- `npm run dev` – Start Vite dev server.  
- `npm run build` – Build the React app for production.  
- `npm run preview` – Preview the production build locally.  

(Exact scripts may vary slightly depending on the tutorial configuration.)  

***
