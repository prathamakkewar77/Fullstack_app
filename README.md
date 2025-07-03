# 🚀 Full‑Stack CRUD Dashboard  
### Next.js · Tailwind · TypeScript · Google Cloud Functions · Firestore · Docker

This Full-Stack application is built using Next.js and incorporates Firebase authentication for sign-in and sign-up functionality, including custom authentication and Google authentication. It also includes CRUD (Create, Read, Update, Delete) operations, login/logout functionality, and utilizes Tailwind CSS for styling.

A production‑ready example of a modern CRUD web app:

- **Frontend** – Next.js 14, Tailwind CSS, TypeScript  
- **Backend** – Google Cloud Functions (HTTP)  
- **Database** – Cloud Firestore  
- **Deployment** – Docker (for local + CI/CD)
- 
## Features

- Firebase authentication for sign-in and sign-up functionality.
- Custom authentication and Google authentication options.
- CRUD operations for managing data.
- Login and logout functionality.
- Integration with Tailwind CSS for styling.

---

## 🖼️ Architecture

```
Frontend (Dockerized)
┌──────────────────────────────────────────────┐
│                                              │
│        Next.js + Tailwind + TypeScript       │
│      ───────────────────────────────────     │
│      |  User Dashboard (CRUD UI)       |     │
│      |  fetch() → Google Cloud APIs    |     │
│      ───────────────────────────────────     │
│                                              │
└───────────────▲──────────────────────────────┘
                │
                │ HTTP Request (REST API)
                ▼
┌──────────────────────────────────────────────┐
│     Google Cloud Functions (Serverless API)  │
│     ───────────────────────────────────────  │
│     | /api/users (POST, GET, PUT, DELETE) |  │
│     ───────────────────────────────────────  │
└───────────────▲──────────────────────────────┘
                │
                │ Firestore SDK Calls
                ▼
┌──────────────────────────────────────────────┐
│            Cloud Firestore (NoSQL DB)        │
│     ───────────────────────────────────────  │
│     | Collection: users                   |  │
│     | Fields: name, email, role, ...      |  │
│     ───────────────────────────────────────  │
└──────────────────────────────────────────────┘
```

---

## ✨ Features

| Category | Highlights |
|----------|------------|
| UI       | Tailwind responsive dashboard, modal forms, toast feedback |
| Code     | Type‑safe models with Zod & TypeScript, ESLint + Prettier |
| API      | RESTful routes: **POST/GET/PUT/DELETE /users** |
| Data     | Real‑time Firestore reads with snapshot listeners |
| DevOps   | Multi‑stage **Dockerfile**, `.dockerignore`, ready for Cloud Run |
| Docs     | Full API spec & usage samples right here ↓ |

---

## 📂 Project Structure

```
root
 ├─ components/       # React UI pieces
 ├─ pages/            # Next.js routes
 │   └─ api/          # (optional) stub API for local fallback
 ├─ firebase/         # Cloud Function source
 ├─ public/           # Assets
 ├─ styles/           # Tailwind global styles
 ├─ Dockerfile
 ├─ .dockerignore / .gitignore
 └─ README.md
```

---

## ⚙️ Prerequisites

| Tool | Version |
|------|---------|
| Node.js | 18 + |
| Docker | 20 + |
| Firebase CLI | latest |
| Google Cloud project | Firestore + Functions enabled |

---

## 🔑 Environment Variables

Create **`.env.local`** (for Next.js) **and/or** **`.env`** (for Docker):

```
# Frontend
NEXT_PUBLIC_API_BASE_URL=https://<region>-<project>.cloudfunctions.net

# Firebase / GCP
FIREBASE_API_KEY=*****
FIREBASE_PROJECT_ID=<project>

```

---

## 🏃‍♀️ Local Development

```bash
# 1. Install deps
npm install

# 2. Run Next.js dev server
npm run dev
# → http://localhost:3000
```

### Local Firestore emulator (optional)

```bash
firebase emulators:start
```

Update `NEXT_PUBLIC_API_BASE_URL` to the emulator URL during testing.

---

## 🐳 Docker Workflow

```bash
# Build image
docker build -t crud-dashboard .

# Run container
docker run --env-file .env -p 3000:3000 crud-dashboard
# → http://localhost:3000
```

---

## 🔄 CRUD Guide (UI)

| Action | How‑to |
|--------|--------|
| **Create** | Click **“Add User”** → fill form → **Save** |
| **Read**   | Dashboard auto‑loads all users on mount |
| **Update** | Row ➜ **✏️ Edit** → change fields → **Save** |
| **Delete** | Row ➜ **🗑️ Delete** → confirm |

All ops hit the Cloud Function endpoints documented below.

---

## 📄 API Reference

_Base URL →_ `https://<region>-<project>.cloudfunctions.net/api`

### Endpoints

| Method | Path | Purpose | Return Codes |
|--------|------|---------|--------------|
| **GET**    | `/users`         | List all users            | `200` |
| **POST**   | `/users`         | Create a new user         | `201`, `400` |
| **GET**    | `/users/:id`     | Fetch single user         | `200`, `404` |
| **PUT**    | `/users/:id`     | Update user               | `200`, `400`, `404` |
| **DELETE** | `/users/:id`     | Delete user               | `204`, `404` |

### Schemas

#### User object

| Field      | Type   | Notes |
|------------|--------|-------|
| `id`       | string | Firestore doc ID |
| `name`     | string | 1–60 chars |
| `email`    | string | Valid email |
| `role`     | string | `"admin"` \| `"user"` |
| `createdAt`| ISO string | auto |
| `updatedAt`| ISO string | auto |

#### Example – Create User

```http
POST /users
Content-Type: application/json

{
  "name": "Asha Rao",
  "email": "asha@example.com",
  "role": "admin"
}
```

`201 Created`

```json
{
  "id": "zH87kJGUT84m",
  "name": "Asha Rao",
  "email": "asha@example.com",
  "role": "admin",
  "createdAt": "2025-07-03T14:45:18.177Z"
}
```

#### Error format

```json
{
  "type": "https://api.example.com/errors/validation",
  "title": "Validation Error",
  "status": 400,
  "detail": "Email is invalid",
  "instance": "/users"
}
```

---

## 🧪 Testing

```bash
npm run test   # Vitest + React Testing Library
npm run lint   # ESLint + Prettier
```

---

## 🚀 Deployment

### 1. Deploy Cloud Functions

```bash
firebase login
firebase deploy --only functions
```

### 2. Deploy frontend to Cloud Run with Docker

```bash
gcloud run deploy dashboard   --source .   --region asia-south1   --set-env-vars "$(cat .env | xargs)"
```

Or push to **Vercel** directly (Next.js native hosting).

---

## 📜 License

[MIT](LICENSE)

---

## 🙋‍♂️ Author

**Pratham Akkewar** • [GitHub](https://github.com/prathamakkewar77)

---

> ⭐ Feel free to star the repo, open issues, or submit PRs!
