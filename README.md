# EduNexus – School Management & Learning Platform

> **One Platform. Every Classroom. Connected.**

EduNexus is a modern, full-stack school management and learning platform designed to bring students, teachers, parents, administrators, and school staff together in one centralized digital environment.

The platform aims to simplify academic administration, improve communication, organize learning resources, and make important academic information accessible to authorized users.

EduNexus is a modernization of an earlier School Management System project, evolving it into a more scalable, interactive, and user-friendly web application using React, Node.js, Express, and PostgreSQL.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [System Architecture](#system-architecture)
- [Database Design](#database-design)
- [Project Structure](#project-structure)
- [User Roles](#user-roles)
- [Core Modules](#core-modules)
- [Development Roadmap](#development-roadmap)
- [Installation and Setup](#installation-and-setup)
- [Environment Variables](#environment-variables)
- [API Documentation](#api-documentation)
- [Security Considerations](#security-considerations)
- [Future Enhancements](#future-enhancements)
- [Screenshots](#screenshots)
- [Contributing](#contributing)

---

## Project Overview

Traditional school management processes often involve manual record keeping, disconnected communication channels, and time-consuming administrative tasks.

EduNexus aims to address these challenges through a centralized web application.

The platform will provide:

- Centralized student and teacher management.
- Digital attendance tracking.
- Academic performance and results management.
- Timetable management.
- Lecture videos, notes, and syllabus access.
- Real-time communication between school members.
- Academic reports and administrative dashboards.

The application will be designed with a responsive interface and a modular backend architecture to support future expansion.

## Objectives

1. Digitize and simplify school management activities.
2. Improve communication between students, teachers, parents, and administrators.
3. Provide convenient access to academic results and student records.
4. Support online learning through lecture videos, notes, and syllabus resources.
5. Simplify attendance tracking and academic report generation.
6. Provide a centralized communication system for all authorized school members.
7. Build a modern, responsive, and maintainable application.

---

## Key Features

### 1. Authentication and Role-Based Access Control

- Secure user registration and login.
- Role-based authorization.
- Protected application routes.
- User profile management.
- Password hashing and secure session/token handling.

Planned user roles:

- Administrator
- Teacher
- Student
- Parent
- Other authorized school staff

### 2. Student Management

- Student registration and profile management.
- Student academic information.
- Parent/student relationship management.
- Student record retrieval.
- Academic history.

### 3. Teacher and Staff Management

- Teacher profiles.
- Faculty information management.
- Department and subject associations.
- Staff-related administrative information.

### 4. Attendance Management

- Digital attendance records.
- Attendance tracking by class and subject.
- Student attendance history.
- Attendance summaries and reports.

### 5. Academic Results Management

- Upload and manage student results.
- View results through authenticated student accounts.
- Academic performance history.
- Generate academic reports.

### 6. Timetable Management

- Class timetable creation.
- Subject and teacher scheduling.
- Timetable access for students and teachers.
- Centralized timetable updates.

### 7. Learning Resource Management

A dedicated learning environment where teachers can share educational materials.

Features:

- Upload lecture videos.
- Share PDF notes and documents.
- Organize resources by subject and course.
- Upload and manage syllabi.
- Allow students to access learning materials.
- Track video-watching progress (planned).

**Video storage strategy:**

Actual video files will be stored separately from PostgreSQL.

PostgreSQL will maintain video metadata, ownership, course associations, and storage references. A dedicated storage service will handle file uploads and retrieval.

Initial development will use local file storage, with the architecture designed to support Amazon S3 later.

### 8. Real-Time Chat and Communication

EduNexus will include a centralized messaging system connecting all authorized users within the same school.

**Core features:**

- One-to-one messaging between any authorized school members.
- Search users by name or role.
- Start conversations with students, teachers, administrators, parents, and other staff.
- Real-time message delivery.
- Persistent message history.
- Message timestamps.
- Read and delivery indicators.
- Online/offline presence (planned).

**Technology:**

- Socket.IO for real-time communication.
- Express.js for application APIs.
- PostgreSQL for persistent message storage.

The chat system will enforce authentication and school-level access restrictions.

### 9. Administrative Dashboard and Reports

- Centralized administrative dashboard.
- Student and teacher statistics.
- Attendance summaries.
- Academic performance reports.
- Overview of school activities.

### 10. Fee Management (Later Milestone)

- Student fee records.
- Payment status tracking.
- Fee history.
- Payment portal integration in a later development phase.

---

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React.js |
| Styling | CSS / Tailwind CSS |
| Backend | Node.js |
| Backend Framework | Express.js |
| Database | PostgreSQL |
| ORM | Prisma |
| Real-Time Communication | Socket.IO |
| API Architecture | REST APIs |
| Authentication | JWT |
| Password Security | bcrypt |
| File Storage | Local storage initially |
| Future Cloud Storage | Amazon S3 |
| Version Control | Git and GitHub |

**Architecture note:** This project uses React, Express, Node.js, and PostgreSQL. It is not a traditional MERN application because it uses PostgreSQL instead of MongoDB.

---

## System Architecture

EduNexus will follow a modular client-server architecture.

```mermaid
flowchart TD
    A[Users: Students, Teachers, Parents, Admins]
    A --> B[React Frontend]

    B --> C[Express REST API]
    B <--> D[Socket.IO Client]

    C --> E[Authentication & Authorization]
    C --> F[Application Modules]
    D <--> G[Socket.IO Server]

    F --> H[(PostgreSQL)]
    G --> H

    F --> I[File Storage Service]
    I --> J[Local Storage]
    I -. Future Migration .-> K[Amazon S3]
```

### Architecture Responsibilities

**React Frontend**
- User interface and navigation.
- Dashboard rendering.
- Forms and data visualization.
- Chat interface.
- Learning resource interface.

**Node.js + Express Backend**
- REST API endpoints.
- Authentication and authorization.
- Business logic.
- Academic data management.
- File upload coordination.

**Socket.IO**
- Real-time messaging.
- Message delivery events.
- Online/offline presence.
- Read receipt events.

**PostgreSQL**
- User records.
- Academic records.
- Attendance.
- Timetables.
- Chat conversations and messages.
- Lecture metadata and video progress.

**File Storage Service**
- Lecture videos.
- PDF notes.
- Uploaded documents.
- Other educational resources.

---

## Database Design

PostgreSQL will serve as the primary relational database.

### Proposed Database Tables

| Module | Tables |
|---|---|
| Authentication | users, roles |
| Student Management | students, parents, student_parents |
| Teacher Management | teachers, departments, subjects |
| Attendance | attendance, attendance_sessions |
| Academics | courses, subjects, results |
| Timetable | timetables, timetable_entries |
| Learning Resources | lectures, lecture_videos, lecture_resources |
| Chat | conversations, conversation_participants, messages, message_receipts |
| Fees | fee_records, payments |
| Progress Tracking | video_progress |

*The final schema will be refined during database design.*

### Chat Database Design

The messaging system will use relational tables.

- `conversations`: Stores conversation information.
- `conversation_participants`: Associates users with conversations.
- `messages`: Stores message content, sender, and timestamp.
- `message_receipts`: Tracks delivery and read status.

This structure supports persistent one-to-one messaging while allowing future expansion.

### Video Database Design

PostgreSQL will store metadata rather than binary video files.

Example fields:

- `video_id`
- `lecture_id`
- `uploaded_by`
- `title`
- `description`
- `storage_key`
- `duration`
- `created_at`

The actual video will reside in the configured file-storage system.

---

## Project Structure

The application will use a modular frontend/backend structure.

```text
EduNexus/
│
├── client/
│   ├── public/
│   │
│   └── src/
│       ├── assets/
│       ├── components/
│       │   ├── common/
│       │   ├── layout/
│       │   ├── dashboard/
│       │   └── chat/
│       │
│       ├── pages/
│       │   ├── auth/
│       │   ├── admin/
│       │   ├── students/
│       │   ├── teachers/
│       │   ├── academics/
│       │   ├── attendance/
│       │   ├── timetable/
│       │   ├── learning/
│       │   └── messages/
│       │
│       ├── hooks/
│       ├── context/
│       ├── services/
│       ├── utils/
│       ├── routes/
│       ├── styles/
│       └── App.jsx
│
├── server/
│   ├── src/
│   │   ├── config/
│   │   ├── controllers/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── services/
│   │   │   ├── auth/
│   │   │   ├── chat/
│   │   │   ├── storage/
│   │   │   └── academic/
│   │   │
│   │   ├── sockets/
│   │   ├── validators/
│   │   ├── utils/
│   │   ├── app.js
│   │   └── server.js
│   │
│   ├── prisma/
│   │   ├── schema.prisma
│   │   └── migrations/
│   │
│   └── uploads/
│
├── docs/
│   ├── architecture/
│   ├── database/
│   └── api/
│
├── screenshots/
│
├── .gitignore
└── README.md
```

---

## User Roles

| Role | Main Responsibilities |
|---|---|
| Administrator | Manage users, school information, academic records, and system settings |
| Teacher | Manage attendance, results, timetables, and learning resources |
| Student | Access academic information, results, timetables, and learning materials |
| Parent | Access permitted student information and academic updates |
| Other Staff | Access features according to assigned permissions |

Messaging will be available to all authorized users, subject to school membership and account permissions.

---

## Development Roadmap

The project will be developed incrementally.

### Phase 1 – Foundation and Architecture
- [ ] Finalize application architecture.
- [ ] Initialize React frontend.
- [ ] Initialize Node.js and Express backend.
- [ ] Configure PostgreSQL.
- [ ] Configure Prisma ORM.
- [ ] Establish frontend/backend communication.
- [ ] Design initial database schema.

### Phase 2 – Authentication and User Management
- [ ] User registration and login.
- [ ] Password hashing.
- [ ] JWT authentication.
- [ ] Role-based authorization.
- [ ] User profile management.
- [ ] Protected routes.

### Phase 3 – Academic Management
- [ ] Student management.
- [ ] Teacher management.
- [ ] Attendance management.
- [ ] Academic results.
- [ ] Timetable management.
- [ ] Administrative dashboard.

### Phase 4 – Learning Management
- [ ] Course and subject organization.
- [ ] Syllabus management.
- [ ] Lecture video uploads.
- [ ] Notes and document uploads.
- [ ] Learning resource access.
- [ ] Video progress tracking.

### Phase 5 – Real-Time Communication
- [ ] Socket.IO integration.
- [ ] User search.
- [ ] One-to-one conversations.
- [ ] Real-time message delivery.
- [ ] Persistent message history.
- [ ] Read receipts.
- [ ] Online/offline status.

### Phase 6 – UI/UX and Quality Improvements
- [ ] Responsive interface.
- [ ] Consistent design system.
- [ ] Loading and error states.
- [ ] Form validation.
- [ ] Backend error handling.
- [ ] Unit and integration testing.
- [ ] Performance improvements.

### Phase 7 – Additional Features and Deployment
- [ ] Fee management.
- [ ] Payment gateway evaluation and integration.
- [ ] Cloud file storage migration.
- [ ] Production deployment.
- [ ] CI/CD pipeline.
- [ ] Application documentation.

---

## Installation and Setup

### Prerequisites

Install the following:

- Node.js and npm.
- PostgreSQL.
- Git.

### 1. Clone the Repository

```bash
git clone https://github.com/ishaan175pathak/CampusFlow.git

cd CampusFlow
```

### 2. Frontend Setup

```bash
cd client

npm install

npm run dev
```

### 3. Backend Setup

```bash
cd server

npm install
```

### 4. Configure Environment Variables

Create a `.env` file inside the server directory.

```env
PORT=5000
DATABASE_URL="postgresql://username:password@localhost:5432/edunexus"

JWT_SECRET=your_secret_key

CLIENT_URL=http://localhost:5173

STORAGE_TYPE=local
UPLOAD_DIR=uploads
```

### 5. Database Setup

After configuring PostgreSQL and Prisma:

```bash
npx prisma generate

npx prisma migrate dev --name init
```

### 6. Start the Backend

```bash
npm run dev
```

*Note: These commands describe the planned development setup. Package scripts and configuration will be finalized during implementation.*

---

## Environment Variables

| Variable | Description |
|---|---|
| PORT | Backend server port |
| DATABASE_URL | PostgreSQL connection string |
| JWT_SECRET | JWT signing secret |
| CLIENT_URL | Frontend application URL |
| STORAGE_TYPE | File storage provider |
| UPLOAD_DIR | Local upload directory |

Never commit `.env` files, database credentials, or production secrets to GitHub.

---

## API Documentation

The backend will expose REST APIs organized by feature.

| Module | Example API Prefix |
|---|---|
| Authentication | `/api/auth` |
| Users | `/api/users` |
| Students | `/api/students` |
| Teachers | `/api/teachers` |
| Attendance | `/api/attendance` |
| Results | `/api/results` |
| Timetable | `/api/timetable` |
| Learning Resources | `/api/learning` |
| Chat | `/api/conversations` |
| Fees | `/api/fees` |

Detailed endpoint documentation will be added as the APIs are implemented.

---

## Security Considerations

- Passwords will be hashed before storage.
- Protected APIs will require authentication.
- Role-based authorization will restrict access to sensitive operations.
- Users will only access resources permitted by their school membership and permissions.
- Chat conversations and messages will be checked against participant membership.
- Uploaded files will be validated for file type and size.
- Sensitive configuration will be stored in environment variables.
- Database queries will use parameterized operations through the ORM.
- Production deployment will use HTTPS and secure configuration.

---

## Future Enhancements

Potential future improvements include:

- Payment gateway integration.
- Advanced academic analytics.
- Automated notifications.
- Parent-teacher communication improvements.
- Cloud-based media delivery.
- More advanced reporting and data visualization.
- Mobile-friendly experience and potential mobile application.

Biometric or face-recognition login is not part of the current development scope.

---

## Screenshots

Screenshots and demonstrations will be added as the application develops.

### Landing Page

![Landing Page](screenshots/landing-page.png)

### Authentication

![Login Page](screenshots/login.png)

### Dashboard

![Dashboard](screenshots/dashboard.png)

### Student Management

![Student Management](screenshots/student-management.png)

### Learning Resources

![Learning Resources](screenshots/learning-resources.png)

### Real-Time Chat

![Chat Application](screenshots/chat.png)

### Attendance Management

![Attendance](screenshots/attendance.png)

---

## Contributing

Contributions, suggestions, and improvements are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Commit your changes.
4. Push the branch.
5. Open a pull request.

---

## Project Information

**Project Name:** EduNexus  
**Category:** Full-Stack Web Application / School Management System  
**Frontend:** React.js  
**Backend:** Node.js + Express.js  
**Database:** PostgreSQL  
**Real-Time Communication:** Socket.IO  
**Repository:** [CampusFlow](https://github.com/ishaan175pathak/CampusFlow)

---

*EduNexus – Making school management simpler, learning more accessible, and communication more connected.*