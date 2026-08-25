# Kerala Paints — Complaint Tracking System

MERN stack app: a public complaint submission form + an admin dashboard
(New / Pending / Solved) for managing them.

## Structure

```
backend/    Express + MongoDB API, JWT admin auth
frontend/   React + Vite app (customer form, track page, admin login + dashboard)
```

## Backend setup

```bash
cd backend
npm install
cp .env.example .env       # then edit .env: MONGO_URI, JWT_SECRET, SEED_ADMIN_*
npm run seed:admin         # creates the first admin login from .env
npm run dev                # starts the API on http://localhost:5000
```

## Frontend setup

```bash
cd frontend
npm install
npm run dev                 # starts on http://localhost:5173, proxies /api to :5000
```

Open `http://localhost:5173/` for the customer complaint form,
`http://localhost:5173/track` to check a complaint by token, and
`http://localhost:5173/admin/login` for the admin dashboard.

## What's placeholder data (edit before going live)

`frontend/src/data/options.js` holds the district, outlet, product, and
quantity dropdown lists. Districts are the real 14 Kerala districts; **outlets,
products, and quantities are placeholders** — swap in the real Kerala Paints
lists there and every dropdown updates automatically.

## How it works

- A customer submits the form → backend validates it, generates a token like
  `KP-2026-0001`, and stores the complaint with status `new`.
- The customer can check progress any time at `/track` using that token.
- The admin logs in and sees three columns — New, Pending, Solved — each
  complaint as a card. Clicking a card opens full details; buttons on the
  card or in the detail view move it between any of the three statuses
  (including moving something already "Solved" back to "Pending", etc.).
  Every status change is recorded in `statusHistory` on the complaint
  document, so there's an audit trail if you need it later.

## Notes

- Admin auth is a single shared login per account (JWT, 8h expiry by
  default). Create more accounts by inserting into the `admins` collection
  or extending `seed/seedAdmin.js`.
- CORS is wide open in `server.js` for local development — lock it down to
  your real frontend origin before deploying.
