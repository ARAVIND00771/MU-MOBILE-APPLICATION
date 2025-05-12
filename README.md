Great — thanks for the update! I’ll mark the APK build as partially completed in the README so that it honestly reflects your progress.

Here is your final, submission-ready README.md — updated with all corrections:

⸻



#  MU Connect – Mahindra University Mobile App

**MU Connect** is a full-stack role-based mobile application developed to enhance communication between students, faculty, and parents at Mahindra University. It features real-time announcements, task submissions, profile management, schedules, and a live location feature for parents (partially implemented).

---

##  Project Documents

- 📄 [SRS – Software Requirements Specification](./docs/GROUP%2038%20SRS.docx)
- 📄 [SOW – Statement of Work](./docs/GROUP%20NO.38%20%5BSOW%5D.docx)
- 📄 [SDD – Software Design Document](./docs/SDD.md)
- 📄 [Problem Statement](./docs/ProblemStatement.md)

---

##  Key Features

###  Student
- View profile, attendance, tasks, announcements
- Upload assignments and view timetable
- Role-based dashboard

###  Faculty
- Post announcements
- View student profiles and submissions
- Manage attendance

###  Parent
- Access student profile, announcements
- View faculty contacts
-  **Live location tracking (partially implemented)**

---

##  Authentication

- Firebase Authentication with role-based access:
  - Student
  - Faculty
  - Parent

---

##  How to Run the Project (Local Setup)

###  Prerequisites

- Node.js (v16+)
- Firebase project setup (Auth + Firestore)
- Git

---

###  Backend Setup

```bash
cd backend
npm install
npm run dev

Backend server runs on http://localhost:5000
 .env and Firebase Admin SDK are already included.

⸻

📱 Frontend Setup

cd frontend
npm install
npm run dev

Frontend runs on http://localhost:5173
 Firebase config is already present in firebase.ts.

⸻

##  Tech Stack

| Layer          | Technology                |
|----------------|---------------------------|
| Frontend       | React + Vite + Tailwind   |
| Backend        | Node.js + Express.js      |
| Authentication | Firebase Auth             |
| Database       | Firebase Firestore        |
| Maps           | Google Maps API           |
| Styling        | Tailwind CSS              |



⸻

 Known Limitations
	•	Parent-student location sync is partially working
	•	APK generation is in progress via EAS build / Expo config
	•	No cloud hosting or Play Store deployment yet

⸻

##  Team Members & Contributions

| Name                | Roll No       | Contributions                                    |
|---------------------|---------------|--------------------------------------------------|
| Kasoju Aravind      | SE22UCSE131   | Firebase Auth, fullstack integration, APK generation |
| P.K.L. Ganesh       | SE22UCSE197   | Student UI, navigation, layout screens           |
| A. Sai Rohan        | SE22UCSE098   | Auth APIs, announcement logic, backend integration |
| Tanush              | SE22UCSE213   | UI/UX designs and Figma wireframes              |
| Sai Snigdha         | SE22UCSE144   | QA testing: login, dashboard, announcements     |
| Pavan Tejas Marri   | SE22UCSE172   | Backend: attendance and Firestore operations    |
| Koushik             | SE22UCSE228   | Deployment setup, GitHub config                 |
| Rithvik             | SE22UCSE199   | QA: Parent module testing and bug reports       |



⸻

##  Project Status

| Feature                        | Status        |
|--------------------------------|---------------|
| Student & Faculty Modules      |  Complete    |
| Parent Module UI               |  Complete    |
| Parent-Student Location Tracking |  Partial     |
| GitHub Push                    |  Done        |
| APK Build                      |  In Progress |



⸻

 Contact

Kasoju Aravind
 se22ucse131@mahindrauniversity.edu.in

⸻



