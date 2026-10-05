# AI Interview Prep

An AI-powered interview preparation platform that helps candidates analyze a target role, identify skill gaps, generate technical and behavioral interview questions, build a preparation plan, and create an ATS-friendly resume.

The application uses a React frontend, a Node.js/Express backend, MongoDB for persistence, and Google Gemini for structured AI-generated interview reports.

## Table of Contents

1. [What this project does](#what-this-project-does)
2. [Features](#features)
3. [Tech Stack](#tech-stack)
4. [Application Flow](#application-flow)
5. [Architecture](#architecture)
6. [AI Interview Report](#ai-interview-report)
7. [Authentication](#authentication)
8. [Resume PDF Generation](#resume-pdf-generation)
9. [Project Structure](#project-structure)
10. [How to Run Locally](#how-to-run-locally)
11. [Environment Variables](#environment-variables)
12. [API Reference](#api-reference)
13. [Security Notes](#security-notes)
14. [Troubleshooting](#troubleshooting)
15. [Future Improvements](#future-improvements)
16. [Credits & Inspiration](#credits--inspiration)
17. [License](#license)

---

## What this project does

The application is designed around a simple interview-preparation workflow.

A candidate provides:

- A resume in PDF format
- A target job description
- A short self-description

The backend processes this information and sends it to the AI service. The generated report includes:

- Candidate-to-job match score
- Technical interview questions
- Behavioral interview questions
- Skill gaps with severity
- A day-wise preparation plan
- Target job title

The generated report is stored in MongoDB so authenticated users can access their interview reports later.

The platform can also generate a tailored resume as a PDF based on the candidate profile and target job description.

---

## Features

### Interview Preparation

- AI-generated interview reports
- Resume-based candidate analysis
- Job-description-based analysis
- Candidate/job match score
- Technical interview questions
- Behavioral interview questions
- Explanation of interviewer intent
- Suggested answer guidance
- Skill-gap identification
- Skill-gap severity levels
- Day-wise preparation roadmap

### User Accounts

- User registration
- User login
- User logout
- Authenticated user sessions
- Protected interview routes
- Password hashing
- JWT-based authentication
- Token blacklist handling

### Resume Tools

- Resume PDF upload
- Resume content extraction
- AI-generated ATS-friendly resume content
- Job-specific resume generation
- PDF resume generation

### Data & Backend

- MongoDB persistence
- REST API architecture
- Request validation
- Cookie-based authentication
- CORS configuration
- Modular controllers, routes, models, and services

---

## Tech Stack

### Primary Stack

| Technology | Purpose |
|---|---|
| React.js | Frontend user interface |
| Node.js | Backend runtime |
| Express.js | REST API and server framework |
| MongoDB | Persistent data storage |

### Supporting Technologies

- Vite
- React Router
- Axios
- SCSS
- Mongoose
- Google Gemini API
- JWT
- bcryptjs
- Multer
- pdf-parse
- Puppeteer
- Zod
- dotenv

---

## Application Flow

```text
                    Candidate
                       |
                       v
             +-------------------+
             |   React Frontend  |
             +---------+---------+
                       |
                       | REST API
                       v
             +-------------------+
             | Express Backend   |
             +---------+---------+
                       |
          +------------+-------------+
          |            |             |
          v            v             v
      MongoDB      Gemini AI      PDF Tools
          |            |             |
          |            v             |
          |     Interview Report     |
          |            |             |
          +------------+-------------+
                       |
                       v
             +-------------------+
             | Interview Results |
             | Preparation Plan  |
             | Resume PDF        |
             +-------------------+
```

---

## AI Interview Report

The interview generation endpoint accepts the candidate's resume, self-description, and target job description.

The AI service returns a structured report containing:

### Match Score

A score from 0–100 representing how closely the candidate profile matches the target role.

### Technical Questions

Each generated technical question includes:

- Question
- Interviewer's intention
- Suggested answer approach

### Behavioral Questions

Behavioral questions are generated with:

- Question
- Interviewer's intention
- Suggested answer approach

### Skill Gaps

Each identified skill gap contains:

- Skill name
- Severity

Severity can be:

```text
low
medium
high
```

### Preparation Plan

The AI generates a day-wise preparation roadmap containing:

- Day number
- Main focus
- Tasks to complete

---

## Authentication

The backend provides authentication endpoints for:

- Registration
- Login
- Logout
- Current-user information

Passwords are hashed before being stored.

Authenticated requests use a JWT stored in a cookie. Protected interview endpoints verify the token before allowing access.

The authentication layer also checks a token blacklist during protected requests.

---

## Resume PDF Generation

The platform can generate a job-specific resume from:

- Existing resume content
- Candidate self-description
- Target job description

The AI service generates structured HTML for the resume.

Puppeteer then renders the HTML into an A4 PDF document.

The generated resume is designed to be:

- Job-specific
- Concise
- Professional
- ATS-friendly

---

## Project Structure

```text
AI-Interview-Prep/
│
├── Backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── database.js
│   │   │
│   │   ├── controllers/
│   │   │   ├── auth.controller.js
│   │   │   └── interview.controller.js
│   │   │
│   │   ├── middlewares/
│   │   │   ├── auth.middleware.js
│   │   │   └── file.middleware.js
│   │   │
│   │   ├── models/
│   │   │   ├── blacklist.model.js
│   │   │   ├── interviewReport.model.js
│   │   │   └── user.model.js
│   │   │
│   │   ├── routes/
│   │   │   ├── auth.routes.js
│   │   │   └── interview.routes.js
│   │   │
│   │   ├── services/
│   │   │   └── ai.service.js
│   │   │
│   │   └── app.js
│   │
│   ├── server.js
│   ├── package.json
│   └── .gitignore
│
├── Frontend/
│   ├── src/
│   │   ├── features/
│   │   │   ├── auth/
│   │   │   └── interview/
│   │   │
│   │   ├── App.jsx
│   │   ├── app.routes.jsx
│   │   ├── main.jsx
│   │   └── style/
│   │
│   ├── public/
│   ├── package.json
│   ├── vite.config.js
│   └── .gitignore
│
└── README.md
```

---

## How to Run Locally

### Prerequisites

Install the following:

- Node.js
- npm
- MongoDB
- Git
- Google Gemini API key

Check Node.js:

```powershell
node --version
```

Check npm:

```powershell
npm --version
```

---

### 1. Clone the repository

```powershell
git clone https://github.com/SDhakrey/AI-Interview-Prep.git
cd AI-Interview-Prep
```

---

### 2. Configure the backend

Move into the backend directory:

```powershell
cd Backend
```

Install dependencies:

```powershell
npm install
```

Create a `.env` file inside `Backend/`.

Example:

```env
MONGO_URI=mongodb://localhost:27017/interview-ai
JWT_SECRET=your_long_random_secret
GOOGLE_GENAI_API_KEY=your_google_genai_api_key
```

Do not commit this file.

---

### 3. Start the backend

From the `Backend` directory:

```powershell
npm run dev
```

The backend runs on:

```text
http://localhost:3000
```

---

### 4. Start the frontend

Open a second PowerShell terminal.

Move to the frontend directory:

```powershell
cd D:\Projects\ai-inteview\interview-ai-yt\Frontend
```

Install dependencies:

```powershell
npm install
```

Start Vite:

```powershell
npm run dev
```

The frontend normally runs on:

```text
http://localhost:5173
```

Open the address shown by Vite in your browser.

---

## Environment Variables

Backend environment variables:

| Variable | Purpose |
|---|---|
| `MONGO_URI` | MongoDB connection string |
| `JWT_SECRET` | Secret used for JWT authentication |
| `GOOGLE_GENAI_API_KEY` | Google Gemini API credential |

Example:

```env
MONGO_URI=mongodb://localhost:27017/interview-ai
JWT_SECRET=replace_with_a_long_random_value
GOOGLE_GENAI_API_KEY=replace_with_your_api_key
```

### Important

Never commit:

```text
.env
API keys
JWT secrets
Database passwords
```

The backend `.gitignore` excludes `.env` and `node_modules`.

---

## API Reference

The backend exposes authentication and interview-related REST endpoints.

### Authentication

#### Register

```http
POST /api/auth/register
```

Creates a new user account.

#### Login

```http
POST /api/auth/login
```

Authenticates an existing user.

#### Logout

```http
GET /api/auth/logout
```

Logs out the current user and invalidates the authentication token.

#### Current User

```http
GET /api/auth/me
```

Returns information about the authenticated user.

---

### Interview

#### Generate Interview Report

```http
POST /api/interview/
```

Creates an interview report using:

- Resume PDF
- Self-description
- Job description

The route requires authentication.

#### Get Interview Report

```http
GET /api/interview/report/:interviewId
```

Returns a specific interview report.

#### Get User Interview Reports

```http
GET /api/interview/
```

Returns interview reports belonging to the authenticated user.

#### Generate Resume PDF

```http
POST /api/interview/resume/pdf/:interviewReportId
```

Generates a tailored resume PDF from an interview report.

---

## Troubleshooting

### Frontend cannot connect to backend

Make sure both applications are running:

```powershell
# Terminal 1
cd Backend
npm run dev
```

```powershell
# Terminal 2
cd Frontend
npm run dev
```

Also verify that the backend is running on port `3000`.

### MongoDB connection error

Check:

- MongoDB is running
- `MONGO_URI` is correct
- The database server is reachable
- Your `.env` file is inside `Backend/`

### Gemini API error

Check that:

```env
GOOGLE_GENAI_API_KEY=your_key
```

is present in `Backend/.env`.

Never place the API key directly inside source code.

### Port already in use

If port `3000` is already occupied, stop the process using it before starting the backend.

---

## Future Improvements

Possible improvements include:

- Interview history dashboard
- Performance analytics
- Interview score trends
- Role-specific interview templates
- Mock interview mode
- Timed interview sessions
- More detailed answer evaluation
- Interview difficulty selection
- Email-based account verification
- Password reset
- Automated backend tests
- API documentation with OpenAPI/Swagger
- Docker support
- CI/CD pipeline
- Production deployment

These are planned improvements and should not be considered current features until implemented.

---

## Credits & Inspiration

This project was developed as a learning and portfolio project using the public **interview-ai-yt** project as an implementation reference and starting point.

Original project:

https://github.com/ankurdotio/interview-ai-yt

The project has been placed in a separate repository for further customization, learning, and development.

---

## License

This repository is intended for educational and portfolio purposes.

