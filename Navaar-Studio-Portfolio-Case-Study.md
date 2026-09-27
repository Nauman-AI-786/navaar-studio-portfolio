# Navaar Studio — Full-Stack Urdu Video Creation Platform

**A creative SaaS product enabling users to turn Urdu poetry and lyrics into shareable short-form videos, with cloud accounts and subscription billing.**

---

## Overview

Navaar Studio is a browser-based video creation tool built for Urdu poetry and lyric video content. Users can type a line of poetry, choose a visual mood, typography style, background, and voice direction, then export a real, downloadable HD video — entirely rendered client-side using the Canvas API and MediaRecorder, with no external video-generation service required.

A second tool, the **Photo + Lyrics Video Maker**, lets users upload their own photos, write multi-line lyrics, add their own audio track, and generate a short lyric-style video with six selectable cinematic layouts (bottom caption, center focus, framed, split-panel, cinematic bars, or randomized).

---

## What I Built

### Frontend
- Fully responsive landing page and interactive studio (HTML/CSS/vanilla JS)
- Real-time canvas-based video rendering and export (`MediaRecorder` API → downloadable `.webm`)
- Dynamic Urdu/English typography switching with RTL-aware layout logic
- Client-side photo slideshow engine with Ken Burns-style zoom, synced captions, and audio track mixing
- Supabase Auth integration (email/password sign up & sign in)

### Backend
- Node.js + Express REST API (`/api/projects`, `/api/checkout`, `/api/webhook`, `/api/subscription`)
- Deployed on **Railway** as a live, publicly accessible service
- Environment-based configuration for secrets and service credentials

### Database & Infrastructure
- **Supabase** (PostgreSQL) for user accounts and data storage
- Custom schema: `projects` and `subscriptions` tables
- **Row Level Security (RLS)** policies ensuring each user can only access their own data
- Auto-generated UUIDs, cascading deletes tied to auth users, and update triggers for timestamp tracking

### Payments (in progress)
- Evaluated Stripe (found unavailable for Pakistan-based accounts) and pivoted to **LemonSqueezy** as a Merchant-of-Record alternative, which handles global tax compliance automatically
- Store created; subscription products (Creator / Studio tiers, monthly & annual pricing) in setup

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, Vanilla JavaScript, Canvas API |
| Auth & Database | Supabase (PostgreSQL, Auth, Row Level Security) |
| Backend | Node.js, Express |
| Hosting (Frontend) | GitHub Pages |
| Hosting (Backend) | Railway |
| Payments | LemonSqueezy (Merchant of Record) |
| Version Control | Git / GitHub |

---

## Architecture

```
User Browser (GitHub Pages)
        │
        ├── Supabase Auth (sign up / sign in)
        │
        ├── Supabase Database (saved projects, RLS-protected)
        │
        └── Express API on Railway
                 │
                 ├── /api/projects  → CRUD for saved video projects
                 ├── /api/checkout  → LemonSqueezy checkout session
                 └── /api/webhook   → subscription status sync
```

---

## Key Engineering Decisions

- **No external AI video service.** Video generation runs entirely in-browser via Canvas + MediaRecorder — zero per-video compute cost.
- **RLS-first data model.** Every table enforces row-level security at the database layer, not just the application layer, so user data stays isolated even if API logic has bugs.
- **Free-tier-first infrastructure.** Backend hosting, database, and frontend hosting were all selected and configured to run at zero fixed cost during early-stage validation.
- **Payment provider pivot.** Adapted the billing integration mid-build after discovering a geographic restriction, choosing a Merchant-of-Record model to avoid manual international tax compliance.

---

## Status

✅ Database schema live with RLS
✅ Backend deployed and reachable via public URL
✅ Frontend connected to live backend
🔄 Payment provider setup in progress (LemonSqueezy)
🔜 End-to-end testing: sign-up → save project → checkout

---

*This project demonstrates end-to-end product ownership: frontend UX and canvas-based media engineering, backend API design, secure multi-tenant database architecture, and third-party payment integration — including adapting the technical approach around a real-world regional platform limitation.*
