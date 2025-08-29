# Mess Expert 🏠🍽️

Mess Expert is a **full-stack web application** designed to simplify the management of mess groups (shared living/eating arrangements). It helps admins and members manage expenses, deposits, meals, and group activities efficiently.

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
* **Meal Management**:

  * Tracks meals by member and meal type (breakfast, lunch, dinner).
* **Dashboard & Reports**:

  * Visual breakdown of deposits vs. expenses.
  * Balance report per member.
  * Monthly summary reports.

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

## 📦 Database Schema

### 1. Users

| Column   | Type         | Description     |
| -------- | ------------ | --------------- |
| id       | SERIAL (PK)  | Unique user ID  |
| fullName | VARCHAR(150) | Full name       |
| username | VARCHAR(50)  | Unique username |
| email    | VARCHAR(100) | Unique email    |
| password | TEXT         | Hashed password |

### 2. Members

| Column         | Type          | Description             |
| -------------- | ------------- | ----------------------- |
| member\_id     | SERIAL (PK)   | Unique member ID        |
| name           | VARCHAR(100)  | Member’s name           |
| phone\_number  | VARCHAR(20)   | Default: 'my number'    |
| image          | TEXT          | Profile image           |
| role           | VARCHAR(20)   | Default: `member`       |
| joining\_date  | DATE          | Date of joining mess    |
| total\_deposit | NUMERIC(10,2) | Track member’s deposits |
| total\_meal    | NUMERIC(10,2) | Track meals count       |
| user\_id       | INT (FK)      | References `users.id`   |

### 3. Mess

| Column    | Type         | Description                    |
| --------- | ------------ | ------------------------------ |
| mess\_id  | SERIAL (PK)  | Unique mess ID                 |
| name      | VARCHAR(100) | Mess name                      |
| location  | TEXT         | Mess location                  |
| admin\_id | INT (FK)     | References `members.member_id` |

### 4. MemberMess (Mapping Table)

| Column     | Type      | Description                    |
| ---------- | --------- | ------------------------------ |
| member\_id | INT (FK)  | References `members.member_id` |
| mess\_id   | INT (FK)  | References `mess.mess_id`      |
| joined\_at | TIMESTAMP | Default: `CURRENT_TIMESTAMP`   |

### 5. Meals

| Column     | Type        | Description                    |
| ---------- | ----------- | ------------------------------ |
| meal\_id   | SERIAL (PK) | Unique meal ID                 |
| member\_id | INT (FK)    | References `members.member_id` |
| mess\_id   | INT (FK)    | References `mess.mess_id`      |
| date       | DATE        | Meal date                      |
| meal\_type | VARCHAR(20) | (Breakfast, Lunch, Dinner)     |

### 6. Deposits

| Column      | Type          | Description                    |
| ----------- | ------------- | ------------------------------ |
| deposit\_id | SERIAL (PK)   | Unique deposit ID              |
| member\_id  | INT (FK)      | References `members.member_id` |
| mess\_id    | INT (FK)      | References `mess.mess_id`      |
| amount      | NUMERIC(10,2) | Deposit amount                 |
| date        | DATE          | Date of deposit                |
| note        | TEXT          | Optional note                  |

### 7. Expenses

| Column      | Type          | Description                    |
| ----------- | ------------- | ------------------------------ |
| expense\_id | SERIAL (PK)   | Unique expense ID              |
| mess\_id    | INT (FK)      | References `mess.mess_id`      |
| member\_id  | INT (FK)      | References `members.member_id` |
| category    | VARCHAR(50)   | Expense category               |
| amount      | NUMERIC(10,2) | Expense amount                 |
| date        | DATE          | Expense date                   |
| description | TEXT          | Details                        |
| is\_settled | BOOLEAN       | Default: `FALSE`               |

### 8. BillSplits

| Column        | Type          | Description                      |
| ------------- | ------------- | -------------------------------- |
| split\_id     | SERIAL (PK)   | Unique split ID                  |
| expense\_id   | INT (FK)      | References `expenses.expense_id` |
| member\_id    | INT (FK)      | References `members.member_id`   |
| split\_amount | NUMERIC(10,2) | Amount assigned to member        |
| status        | VARCHAR(20)   | Default: `pending`               |

### 9. Dues

| Column      | Type          | Description                      |
| ----------- | ------------- | -------------------------------- |
| due\_id     | SERIAL (PK)   | Unique due ID                    |
| member\_id  | INT (FK)      | References `members.member_id`   |
| expense\_id | INT (FK)      | References `expenses.expense_id` |
| amount\_due | NUMERIC(10,2) | Due amount                       |
| due\_date   | DATE          | When due is expected             |

### 10. Payments

| Column          | Type          | Description                    |
| --------------- | ------------- | ------------------------------ |
| payment\_id     | SERIAL (PK)   | Unique payment ID              |
| member\_id      | INT (FK)      | References `members.member_id` |
| transaction\_id | INT           | External ref (if needed)       |
| amount\_paid    | NUMERIC(10,2) | Payment amount                 |
| paid\_on        | DATE          | Payment date                   |
| payment\_method | VARCHAR(50)   | e.g., Cash, Mobile Banking     |

### 11. Notifications

| Column           | Type        | Description                    |
| ---------------- | ----------- | ------------------------------ |
| notification\_id | SERIAL (PK) | Unique notification ID         |
| member\_id       | INT (FK)    | References `members.member_id` |
| message          | TEXT        | Notification text              |
| date\_time       | TIMESTAMP   | Default: `CURRENT_TIMESTAMP`   |
| status           | VARCHAR(20) | Default: `unread`              |
| type             | VARCHAR(50) | e.g., Deposit, Expense, Alert  |

### 12. MonthlyReports

| Column         | Type          | Description                  |
| -------------- | ------------- | ---------------------------- |
| report\_id     | SERIAL (PK)   | Unique report ID             |
| mess\_id       | INT (FK)      | References `mess.mess_id`    |
| month          | VARCHAR(20)   | e.g., July 2025              |
| total\_meals   | NUMERIC(10,2) | Total meals                  |
| total\_expense | NUMERIC(10,2) | Total expenses               |
| generated\_on  | TIMESTAMP     | Default: `CURRENT_TIMESTAMP` |

### 13. Market Schedule

| Column       | Type        | Description               |
| ------------ | ----------- | ------------------------- |
| market\_id   | SERIAL (PK) | Unique market ID          |
| mess\_id     | INT (FK)    | References `mess.mess_id` |
| user\_id     | INT (FK)    | References `users.id`     |
| market\_date | DATE        | Default: `CURRENT_DATE`   |

### 14. Personal Deposits

| Column        | Type          | Description             |
| ------------- | ------------- | ----------------------- |
| deposit\_id   | SERIAL (PK)   | Unique ID               |
| user\_id      | INT (FK)      | References `users.id`   |
| amount        | NUMERIC(10,2) | Deposit amount          |
| description   | TEXT          | Note                    |
| deposit\_date | DATE          | Default: `CURRENT_DATE` |

### 15. Personal Expenses

| Column        | Type          | Description             |
| ------------- | ------------- | ----------------------- |
| expense\_id   | SERIAL (PK)   | Unique ID               |
| user\_id      | INT (FK)      | References `users.id`   |
| amount        | NUMERIC(10,2) | Expense amount          |
| description   | TEXT          | Note                    |
| expense\_date | DATE          | Default: `CURRENT_DATE` |

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
