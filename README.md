#  Mahindra University Mobile Application

This is a cross-platform mobile application built for Mahindra University to enhance communication between students, parents, and faculty. The app provides university-related information, real-time geo-location tracking, and announcement features — all within a secure and user-friendly interface.

## Project Documentation
- [Problem Statement](./docs/ProblemStatement.md)
- [Software Requirements Specification (SRS)](./docs/SRS.md)
- [Software Design Document (SDD)](./docs/SDD.md)

## Project Objectives
1. Create a secure communication platform for students, parents, and faculty
2. Implement real-time location tracking for student safety
3. Develop an efficient announcement and notification system
4. Provide a user-friendly interface for all stakeholders
5. Ensure data security and privacy compliance

## Team Members & Responsibilities
- [Team Member 1]: Frontend Development & UI/UX
- [Team Member 2]: Backend Development & Firebase Integration
- [Team Member 3]: Authentication & Security
- [Team Member 4]: Location Tracking & Maps Integration
- [Team Member 5]: Testing & Documentation

## Features

-  **User Authentication**  
  Secure login using Firebase Authentication with role-based access for:
  - Students
  - Parents
  - Faculty

-  **University Information Module**  
  Displays essential university details like departments, events, and courses.

-  **Parent Tracking System**  
  Allows parents to view their child's live location on campus using Google Maps API.

-  **Faculty Broadcast System**  
  Faculty members can send announcements and notifications to students.

-  **Push Notifications**  
  Get real-time alerts about important updates and events.

-  **Smooth UI/UX**  
  Clean, responsive, and intuitive user interface built with React Native.

## Tech Stack

| Layer       | Technology                  |
|-------------|------------------------------|
| Frontend    | React Native (Expo)          |
| Backend     | Node.js + Express.js         |
| Database    | Firebase Firestore           |
| Auth        | Firebase Authentication      |
| Maps        | Google Maps API              |

## Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/mahindra-university-app.git
cd mahindra-university-app
```

### 2. Firebase Setup
- Create a Firebase project at [Firebase Console](https://console.firebase.google.com/).
- Enable Firestore and Authentication (Email/Password or other providers as needed).
- Download the Firebase Admin SDK service account key and place it as `backend/service-account.json` (do **not** commit this file to version control).
- Update Firestore rules and indexes as needed (see `firestore.rules` and `firestore.indexes.json`).

### 3. Install Dependencies
#### Frontend
```bash
npm install
```
#### Backend
```bash
cd backend
npm install
```

### 4. Run the App
#### Frontend (React Native)
```bash
npm run dev
```
#### Backend (Express.js)
```bash
cd backend
npm run dev
```

## System Architecture
The application follows a client-server architecture with the following components:
1. **Frontend**: React Native mobile app
2. **Backend**: Express.js server
3. **Database**: Firebase Firestore
4. **Authentication**: Firebase Auth
5. **Real-time Updates**: Firebase Realtime Database
6. **Maps Integration**: Google Maps API

## Security Measures
- JWT-based authentication
- Role-based access control
- Secure API endpoints
- Encrypted data transmission
- Protected service account credentials

## Notes
- The project uses **Firebase Firestore** for all backend data storage
- Do **not** share your `service-account.json` file publicly. Add it to `.gitignore`
- For Firestore data structure and example documents, see the backend code and Firestore Console
- For any issues, check the backend/README.md for more details on API endpoints and environment variables

## Evaluation Information
This project was developed as part of the Software Engineering course at Mahindra University. The implementation follows best practices in software development, including:
- Agile development methodology
- Continuous integration and testing
- Documentation-driven development
- Security-first approach
- Scalable architecture design

   