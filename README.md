# 💡 Idea Board

An internal idea submission and tracking forum — built as a **single HTML file** with no backend, no build step, and no dependencies to install. Drop it in any static host and it just works.

---

## What it does

Idea Board gives your team a central place to submit, discuss, and track ideas from first spark to launch. Admins review and approve submissions, the whole team votes on what matters most, and everyone can follow ideas through the full lifecycle.

![Status flow: Pending → Submitted → In Review → Planned → In Progress → Launched (or Declined)](https://img.shields.io/badge/status%20flow-Pending%20→%20Launched-blue)

---

## Features

### For everyone
- **Submit ideas** — title, description, owner, tags, and target date
- **Vote** on any approved idea to surface the best ones
- **Similarity detection** — Jaccard algorithm flags near-duplicate submissions before you post
- **Tag filtering** — browse ideas by custom topic tags
- **Idea detail modal** — full description, vote count, status, and submitter info
- **Edit your own ideas** — submitters can update their own entries after submission
- **Notifications** — bell icon with dropdown + full notifications page
  - Upvoted: someone voted on your idea
  - Approved / Declined: admin decision on your submission
  - Status changed: your idea moved to a new stage

### For admins
- **Pending queue** — review, edit, approve, or decline submissions before they go public
- **Status management** — move any idea through the lifecycle: Submitted → In Review → Planned → In Progress → Launched
- **Edit any idea** — including status field, visible only to admins
- **Analytics dashboard** — total ideas, votes, tags, status breakdown
- **New idea alerts** — admins receive a notification whenever a new idea is submitted

### Auth
- **Okta SSO** (PKCE flow, sessionStorage token storage) — for real deployments
- **Local credential fallback** — for demo/dev use without Okta

---

## Getting started

### 1. Clone or download

```bash
git clone https://github.com/brianneidhardtclz/idea-board.git
cd idea-board
```

Or just download `idea-forum.html` directly — it's the only file you need.

### 2. Configure Okta (optional)

Open `idea-forum.html` and find the `CONFIG` block near the top of the `<script>` tag:

```js
const CONFIG = {
  okta: {
    issuer:    'https://YOUR_OKTA_DOMAIN.okta.com/oauth2/default',
    clientId:  'YOUR_CLIENT_ID',
    redirectUri: window.location.origin + window.location.pathname,
    scopes:    ['openid', 'profile', 'email', 'groups'],
    pkce:      true,
    tokenManager: { storage: 'sessionStorage' }
  },
  adminGroup: 'Admins'
};
```

Replace `YOUR_OKTA_DOMAIN` and `YOUR_CLIENT_ID` with your actual Okta application values. Make sure your Okta app has the redirect URI for wherever you're hosting the file.

> **Not using Okta yet?** Leave the config as-is and use the local login below — the setup banner will appear on the login screen as a reminder.

### 3. Open in a browser

You can open the file directly:

```bash
open idea-forum.html
```

Or serve it from any static host (GitHub Pages, S3, Netlify, etc.).

---

## Logging in

### Okta SSO
Click **"Sign in with Okta"** on the login screen. Users in the `Admins` group in Okta will automatically receive the Admin role.

### Local credentials (dev / demo)
Use the username/password form below the Okta button.

| Username | Password | Role |
|----------|----------|------|
| `ADMIN` | `SavingsMeow!` | Admin |

> To add more local users, edit the `LOCAL_USERS` array in the source code.

---

## User roles

| Feature | User | Admin |
|---------|------|-------|
| Submit ideas | ✅ | ✅ |
| Vote on ideas | ✅ | ✅ |
| Edit own ideas | ✅ | ✅ |
| Edit any idea | ❌ | ✅ |
| Change idea status | ❌ | ✅ |
| Approve / decline submissions | ❌ | ✅ |
| View pending queue | ❌ | ✅ |
| View analytics | ❌ | ✅ |
| New idea notifications | ❌ | ✅ |

---

## Idea lifecycle

```
[Submitted] ──▶ [In Review] ──▶ [Planned] ──▶ [In Progress] ──▶ [Launched]
                                                                     │
                                                              [Declined]
```

All new submissions start as **Pending** and only become visible to non-admin users once an admin approves them (moves them to **Submitted**).

---

## Technical notes

- **No backend** — all data lives in memory for the current session; refreshing resets it
- **No build step** — vanilla HTML, CSS, and JavaScript in a single file
- **Okta auth** — uses [okta-auth-js v7.8.1](https://github.com/okta/okta-auth-js) loaded from CDN (PKCE flow)
- **Similarity detection** — Jaccard index on tokenized descriptions, with stop-word filtering
- **Notifications** — per-user in-memory array, triggered by votes, status changes, and new submissions

---

## Project structure

```
idea-board/
├── idea-forum.html   ← the entire application
└── .gitignore
```

---

## License

Internal use — CloudZero
