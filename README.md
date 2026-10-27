# PlayOn Sports Fest 2027 — MERN Tournament Registration Website

Web Programming Lab · Mini Project (Experiment 9)

A full-stack website where players sign up, register teams for sports tournaments, and send feedback. Organisers get an admin dashboard to approve or delete teams.

**Stack:** MongoDB · Express.js · React.js (Vite) · Node.js · Bootstrap 5 · HTML5 · CSS3 · JavaScript

---

## Features

| Requirement | Where it is |
|---|---|
| Home page | `/` — hero, live countdown, stats strip, event cards, how-it-works, FAQ accordion |
| Form 1 (with validation) | `/login` — Sign up / Log in |
| Form 2 (with validation) | `/register` — Team registration (player boxes change with sport) |
| Form 3 (with validation) | `/feedback` — Feedback & contact with star rating |
| Database | MongoDB (Atlas) via Mongoose — `users`, `events`, `teams`, `feedbacks` collections |
| Extra | `/events` filters & sort, `/my-teams`, `/admin` dashboard (search, approve, delete) |

Validation runs **twice**: in the browser (instant red/green feedback) and again on the server (`server/validators.js`), because client checks can be bypassed.

---

## Project structure

```
sportsfest/
├── package.json          # server deps + build/start scripts
├── .env.example          # copy to .env
├── server/
│   ├── server.js         # Express app, MongoDB connection, serves React build
│   ├── seed.js           # inserts the 5 events on first run
│   ├── validators.js     # server-side validation rules
│   ├── middleware/auth.js
│   ├── models/           # User, Event, Team, Feedback (Mongoose schemas)
│   └── routes/           # auth, events, teams, feedback
└── client/               # React app (Vite)
    ├── index.html
    └── src/
        ├── App.jsx       # routes
        ├── api.js        # fetch wrapper
        ├── validation.js # client-side validation rules
        ├── components/   # Navbar, Footer, Countdown, Field, EventCard, Toast
        └── pages/        # Home, Events, AuthPage, RegisterTeam, Feedback, MyTeams, Admin
```

## API endpoints

| Method | Endpoint | Access | Purpose |
|---|---|---|---|
| POST | `/api/auth/signup` | public | create account |
| POST | `/api/auth/login` | public | log in, returns JWT |
| GET | `/api/events` | public | events + live slots left |
| POST | `/api/teams` | logged in | register a team |
| GET | `/api/teams/mine` | logged in | my teams |
| GET | `/api/teams?search=&sport=` | admin | all teams |
| PATCH | `/api/teams/:id/approve` | admin | approve team |
| DELETE | `/api/teams/:id` | admin | delete team |
| POST | `/api/feedback` | public | send feedback |
| GET | `/api/feedback/summary` | public | average rating |
| GET | `/api/feedback` | admin | all feedback |

---

## Run it on your laptop

Needs Node.js 18+.

1. Create the database (step A below) and copy the connection string.
2. In the `sportsfest` folder, copy `.env.example` to `.env` and fill in `MONGO_URI` and `JWT_SECRET`.
3. Run:
   ```bash
   npm install
   npm run build      # installs and builds the React app
   npm start          # http://localhost:5000
   ```
   For live-reload while editing the frontend, run `npm run dev` in one terminal and `cd client && npm run dev` in another (opens http://localhost:5173).

4. Sign up using the `ADMIN_EMAIL` from `.env` to get the admin dashboard.

---

## Deploy live (free): MongoDB Atlas + Render

### A. MongoDB Atlas (database)
1. Go to https://www.mongodb.com/cloud/atlas and sign up.
2. Create a **free M0 cluster**.
3. **Database Access** → Add user → username + password (save them).
4. **Network Access** → Add IP → **Allow access from anywhere** (`0.0.0.0/0`) — needed for Render.
5. **Connect** → Drivers → copy the string, it looks like
   `mongodb+srv://USER:PASSWORD@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority`
   Put the database name after `.net/`: `...mongodb.net/playon?retryWrites=true&w=majority`

### B. GitHub
1. Create a new repository and upload the `sportsfest` folder contents (the `.gitignore` keeps `node_modules` and `.env` out).

### C. Render (web server)
1. Go to https://render.com, sign in with GitHub.
2. **New → Web Service** → pick your repo.
3. Settings:
   - **Runtime:** Node
   - **Build Command:** `npm install && npm run build`
   - **Start Command:** `npm start`
   - **Instance type:** Free
4. **Environment variables:** add `MONGO_URI`, `JWT_SECRET`, `ADMIN_EMAIL` (same as your `.env`). Do not add `PORT` — Render sets it.
5. Click **Create Web Service**. After a few minutes you get a URL like `https://playon-fest.onrender.com`.
6. Open it, sign up with your `ADMIN_EMAIL`, and test all three forms.

> Free Render services sleep after 15 minutes idle; the first visit after that takes ~30–50 seconds to wake. Open the site a minute before your demo.
