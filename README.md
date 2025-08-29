# Mess Expert 🏠🍽️

Mess Expert is a **full-stack web application** designed to simplify the management of mess groups (shared living/eating arrangements). It helps admins and members manage expenses, deposits, and group activities efficiently.

---

## 📌 Table of Contents

* [Introduction](#introduction)
* [Features](#features)
* [Tech Stack](#tech-stack)
* [System Architecture](#system-architecture)
* [Database Schema](#database-schema)
* [Project Setup](#project-setup)
* [API Endpoints](#api-endpoints)
* [Future Scope](#future-scope)
* [Contributors](#contributors)

---

## 🔹 Introduction

Managing a mess (shared living/food expense system) can be challenging when multiple members contribute to expenses, deposits, and bills. **Mess Expert** aims to:

* Provide a centralized platform for mess admins and members.
* Ensure transparency in transactions and balances.
* Simplify daily record-keeping for food, rent, and utilities.

---

## ✨ Features

* **User Authentication**: Secure login/signup (JWT-based).
* **Role Management**:

  * Admins can create mess groups, add/remove members.
  * Members can view their mess details.
* **Mess Groups**:

  * A user can own multiple mess groups.
  * Each mess group is linked via `mess_id`.
* **Deposit Management**:

  * Members can add deposits.
  * Admins can view/update total deposits.
* **Expense Tracking**:

  * Records daily/weekly/monthly expenses.
  * Splits bills fairly among members.
* **Dashboard & Reports**:

  * Visual breakdown of deposits vs. expenses.
  * Balance report per member.

---

## 🛠️ Tech Stack

**Frontend:** React, Redux Toolkit, TailwindCSS
**Backend:** Node.js, Express.js
**Database:** PostgreSQL (with Supabase)
**Authentication:** JWT (JSON Web Tokens)
**Hosting/Deployment:** Vercel (frontend), Render/Heroku (backend)

---

## 🏗️ System Architecture

```
React (Frontend) <--> Express API (Backend) <--> PostgreSQL (Database)
```

* The frontend interacts with the backend using REST APIs.
* The backend implements the **MVC pattern**.
* Supabase/PostgreSQL stores persistent data.

---

## 🗄️ Database Schema

### Users Table

| Column   | Type | Description     |
| -------- | ---- | --------------- |
| id       | UUID | Primary key     |
| name     | TEXT | User's name     |
| email    | TEXT | Unique email    |
| password | TEXT | Hashed password |

### Mess Table

| Column    | Type | Description           |
| --------- | ---- | --------------------- |
| id        | UUID | Primary key           |
| name      | TEXT | Mess group name       |
| admin\_id | UUID | References `users.id` |

### Members Table

| Column   | Type | Description           |
| -------- | ---- | --------------------- |
| id       | UUID | Primary key           |
| mess\_id | UUID | References `mess.id`  |
| user\_id | UUID | References `users.id` |

### Deposits Table

| Column    | Type    | Description           |
| --------- | ------- | --------------------- |
| id        | UUID    | Primary key           |
| user\_id  | UUID    | References `users.id` |
| mess\_id  | UUID    | References `mess.id`  |
| amount    | NUMERIC | Deposit amount        |
| createdAt | DATE    | Timestamp             |

### Expenses Table

| Column    | Type    | Description          |
| --------- | ------- | -------------------- |
| id        | UUID    | Primary key          |
| mess\_id  | UUID    | References `mess.id` |
| title     | TEXT    | Expense title        |
| amount    | NUMERIC | Expense amount       |
| createdAt | DATE    | Timestamp            |

---

## ⚡ Project Setup

### 1. Clone Repository

```bash
git clone https://github.com/yourusername/mess-expert.git
cd mess-expert
```

### 2. Backend Setup

```bash
cd backend
npm install
cp .env.example .env   # configure database + JWT_SECRET
npm run dev
```

### 3. Frontend Setup

```bash
cd frontend
npm install
npm start
```

---

## 🌐 API Endpoints

### Auth

* `POST /api/auth/register` → Register a new user
* `POST /api/auth/login` → Login and receive JWT

### Mess Groups

* `POST /api/mess` → Create a new mess group (admin)
* `GET /api/mess/:id` → Get mess details

### Members

* `POST /api/members` → Add member to mess group
* `GET /api/members/:mess_id` → List mess members

### Deposits

* `POST /api/deposits` → Add deposit
* `GET /api/deposits/:mess_id` → Get all deposits in a mess

### Expenses

* `POST /api/expenses` → Add expense
* `GET /api/expenses/:mess_id` → Get all expenses

---

## 🚀 Future Scope

* Meal management system (track daily meals per member)
* Automated bill splitting & balance calculations
* Notifications (email/SMS) for low balance
* AI-based monthly budget prediction
* Mobile app (React Native)

---

## 👨‍💻 Contributors

* **Your Name** – Full Stack Developer
* \[Add your teammates if any]

---

📢 *Mess Expert is built to simplify group living and ensure financial transparency among members.*
