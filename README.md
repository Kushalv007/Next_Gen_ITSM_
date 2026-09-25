# Next-Gen ITSM — Enterprise IT Service Management Platform

A modern, production-grade IT Service Management (ITSM) platform built with **React**, **Vite**, **Tailwind CSS**, **Node.js/Express**, **TypeScript**, and **Prisma ORM (PostgreSQL)**.

---

## ⚡ Zero-Setup Quick Start: Run with Just ONE Command

You can run the entire platform by simply typing:

```bash
npm run dev
```

That's it! Nothing else needed.

The intelligent runner (`scripts/dev.js`) automatically:
1. **Checks & installs all dependencies** for root, backend, and frontend if missing.
2. **Configures `.env`** files from templates automatically.
3. **Auto-detects or starts PostgreSQL** (connects to running instance or starts local dev cluster).
4. **Synchronizes Prisma schema & seeds default accounts** (incidents, service requests, catalog items, knowledge base).
5. **Spawns Frontend & Backend concurrently** with live reloading.

- **Frontend Application**: [http://localhost:5173](http://localhost:5173)
- **Backend API**: [http://localhost:4000/api](http://localhost:4000/api)

### 🔑 Default Demo Accounts:
| Role | Email | Password |
|---|---|---|
| **System Administrator** | `admin@itsm.com` | `admin123` |
| **IT Technician / Agent** | `tech@itsm.com` | `tech123` |
| **Employee / End User** | `user@itsm.com` | `user123` |

---

## 🐳 Alternative: Run Anywhere with Docker

If you prefer containers:

```bash
docker compose up --build
```

- **Unified Web App**: [http://localhost:4000](http://localhost:4000)
- **Database**: PostgreSQL 16 Alpine on `localhost:5432`

To stop:
```bash
docker compose down
```

---

## 🚀 Cloud Deployment Options

### Option A: 1-Click Deployment on Render.com (Recommended)
This repository contains a pre-configured [`render.yaml`](./render.yaml) blueprint:
1. Push this project to GitHub.
2. In [Render Dashboard](https://dashboard.render.com), click **New +** → **Blueprint**.
3. Connect your repository. Render will automatically spin up:
   - A managed PostgreSQL database.
   - The unified Web Service (builds frontend, pushes Prisma schema, runs seed, and serves the app).
4. Done!

### Option B: Deploy to Railway / Fly.io / VPS with Docker
You can deploy directly using the root [`Dockerfile`](./Dockerfile):
- **Railway**: Connect your GitHub repo, add a PostgreSQL plugin, and set `DATABASE_URL`. Railway automatically detects the `Dockerfile` and builds the production container.
- **Fly.io**: Run `fly launch` in this directory.

### Option C: Split Deployment (Vercel Frontend + Render/Railway Backend)
- **Backend (Render / Railway / Fly.io)**:
  - Root directory: `backend`
  - Build command: `npm install && npm run build`
  - Start command: `npm run db:push && (npm run seed || true) && npm start`
  - Set `FRONTEND_URL` to your Vercel domain.
- **Frontend (Vercel / Netlify / Cloudflare Pages)**:
  - Root directory: `frontend`
  - Build command: `npm run build`
  - Output directory: `dist`
  - Set `VITE_API_URL` to your backend URL (e.g., `https://your-backend.onrender.com/api`).

---

## 📂 Project Structure

```
vibe/
├── scripts/
│   └── dev.js                # All-in-one orchestrator (auto-deps, auto-db, auto-seed, concurrent run)
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma     # Complete enterprise schema (Incidents, Requests, KB, Assets, etc.)
│   │   └── seed.ts           # Enterprise demo seeder
│   ├── src/
│   │   ├── controllers/      # REST API route controllers
│   │   ├── middleware/       # Auth, RBAC, error handling, validation
│   │   ├── routes/           # Express API routers
│   │   └── index.ts          # Express server with SPA static serving
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── api/              # Axios API client with auto baseURL
│   │   ├── components/       # UI layout, Navbar, Sidebar, Modals
│   │   ├── context/          # Auth and State contexts
│   │   ├── pages/            # Incidents, Service Catalog, Requests, Knowledge, Problems, Changes, Assets
│   │   └── App.tsx           # Declarative React Router routes
│   └── package.json
├── Dockerfile                # Production multi-stage Docker build
├── docker-compose.yml        # Local full-stack container environment
├── render.yaml               # 1-Click Render blueprint
└── package.json              # Unified root scripts
```
