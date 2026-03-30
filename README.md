# 🚀 Thursday Build Initiative

An internal idea submission and tracking platform — built as a **single HTML file** with no backend, no build step, and no dependencies to install. Drop it on any static host and it just works.

---

## What it does

Thursday Build Initiative gives your team a central place to submit ideas for the weekly Thursday Build meeting, track them from first spark through development and deployment, and engage the community with feature voting on live apps. IT/Security gets automatic alerts when an app is approved for development, ensuring compliance tracking from day one.

---

## Features

### For everyone
- **Submit ideas** — title, description, owner, tags, and target date
- **Vote** on any idea to surface what the community wants most
- **Similarity detection** — Jaccard algorithm flags near-duplicate submissions before you post
- **Tag filtering** — browse by topic
- **Idea detail modal** — full description, vote count, status, and submitter info
- **Edit your own ideas** — update after submission
- **Notifications** — bell icon with dropdown for votes, approvals, and status changes

### For Build Owners
- **Thursday Pipeline** — kanban view of ideas moving through Thursday meeting stages
- **Select for Thursday** — pick which ideas get presented at the meeting
- **Mark as Presented** — advance ideas after the meeting
- **Approve for Dev / Decline** — decide which presented ideas go to development

### For IT / Security
- **IT Compliance Tracker** — table of all apps entering or in development
- **Full review modal** — security owner, hosting info, deployment details
- **7-item compliance checklist** — data handling, access controls, SSO, encryption (rest & transit), vulnerability scan, incident response
- **Automatic notification** — IT is alerted the moment a Build Owner approves an idea for development

### For Admins
- **All Build Owner capabilities**
- **All IT capabilities**
- **Edit any idea** including status
- **Analytics dashboard** — total ideas, votes, tags, status breakdown, top voted ideas, deployed apps

### Apps (everyone)
- **Deployed Apps gallery** — browse live internal apps
- **Per-app feature requests** — submit and vote on features for each deployed app
- **Community voting** — upvote feature requests to surface priorities

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
    issuer:         'https://YOUR_OKTA_DOMAIN.okta.com/oauth2/default',
    clientId:       'YOUR_CLIENT_ID',
    redirectUri:    window.location.origin + window.location.pathname,
    scopes:         ['openid', 'profile', 'email', 'groups'],
    pkce:           true,
    tokenManager:   { storage: 'sessionStorage' }
  },
  adminGroup:      'Admins',
  buildOwnerGroup: 'BuildOwners',
  itGroup:         'ITSecurity',
  slackWebhook:    ''    // optional: Slack incoming webhook URL for IT alerts
};
```

Replace `YOUR_OKTA_DOMAIN` and `YOUR_CLIENT_ID` with your Okta application values. Users in the `Admins`, `BuildOwners`, and `ITSecurity` Okta groups will automatically receive the corresponding roles.

> **Not using Okta yet?** Leave the config as-is and use the local login credentials below.

### 3. Open in a browser

```bash
open idea-forum.html
```

Or serve from any static host (GitHub Pages, S3, Netlify, etc.).

---

## Logging in

### Okta SSO
Click **"Sign in with Okta"** on the login screen.

### Local credentials (dev / demo)

| Username | Password | Role |
|----------|----------|------|
| `ADMIN` | `SavingsMeow!` | Admin |
| `OWNER` | `BuildOwner1!` | Build Owner |
| `IT` | `ITSecurity1!` | IT / Security |
| `USER` | `User1234!` | Regular User |

> To add more local users, edit the `LOCAL_USERS` array in the source code.

---

## User roles

| Feature | User | Build Owner | IT / Security | Admin |
|---------|------|-------------|---------------|-------|
| Submit ideas | ✅ | ✅ | ✅ | ✅ |
| Vote on ideas | ✅ | ✅ | ✅ | ✅ |
| Edit own ideas | ✅ | ✅ | ✅ | ✅ |
| Browse deployed apps | ✅ | ✅ | ✅ | ✅ |
| Vote on feature requests | ✅ | ✅ | ✅ | ✅ |
| Submit feature requests | ✅ | ✅ | ✅ | ✅ |
| Thursday Pipeline view | ❌ | ✅ | ❌ | ✅ |
| Select / present / approve ideas | ❌ | ✅ | ❌ | ✅ |
| IT Compliance Tracker | ❌ | ❌ | ✅ | ✅ |
| Run security checklists | ❌ | ❌ | ✅ | ✅ |
| Edit any idea | ❌ | ❌ | ❌ | ✅ |
| Analytics dashboard | ❌ | ❌ | ❌ | ✅ |

---

## Idea lifecycle

```
[Submitted] ──▶ [Selected for Thu] ──▶ [Presented] ──▶ [Approved for Dev] ──▶ [In Development] ──▶ [Deployed]
                                                               │
                                                          [Declined]
```

When an idea moves to **Approved for Dev**, IT is automatically notified (in-app + optional Slack webhook) and a compliance record is created in the IT Compliance Tracker.

---

## IT Compliance checklist

Each app that enters development gets a compliance record with:

- Data handling & privacy reviewed
- Access controls & permissions defined
- SSO / authentication method documented
- Encryption at rest confirmed
- Encryption in transit confirmed
- Vulnerability scan completed
- Incident response plan documented

IT can also capture: security owner, compliance status (Pending / In Review / Approved / Rejected), deployment URL, hosting environment, and freeform review notes.

---

## Technical notes

- **No backend** — all data lives in memory for the current session; refreshing resets it
- **No build step** — vanilla HTML, CSS, and JavaScript in a single file
- **Okta auth** — uses [okta-auth-js v7.8.1](https://github.com/okta/okta-auth-js) loaded from CDN (PKCE flow)
- **Similarity detection** — Jaccard index on tokenized descriptions, with stop-word filtering
- **Notifications** — per-user in-memory array, triggered by votes, status changes, and new submissions
- **Slack webhook** — optional outbound POST when an idea is approved for development

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
