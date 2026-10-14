# Reliant Advisory: dynamic website + backend

One Node.js/Express server that serves the website and stores its data in SQLite.

## What is dynamic now
| Content | Where you manage it | Shown on the website |
|---|---|---|
| Client organizations | Admin > Clients (add, edit, hide, delete, **bulk CSV upload**) | Clients page + the client count on the home page |
| Articles / Insights | Admin > Insights (write, save as draft or publish) | Insights page (`#/insights`, linked from the footer) |
| Enquiries from the contact form | Admin > Enquiries (status tracking, **CSV export**) | Not public |
| Page views | Admin > Analytics (last 30 days) | Not public |

If the API is unreachable, the site falls back to its built-in client list.

## Run locally
```
npm install
cp .env.example .env     # set ADMIN_TOKEN; add SMTP_USER / SMTP_PASS for email alerts
npm start                # site: http://localhost:3000   admin: http://localhost:3000/admin.html
```

## Uploading data later
- **One client:** Admin > Clients > "Add a client".
- **Many clients:** Admin > Clients > "Bulk upload (CSV)". Format, one per line: `name,sector` (a header row is skipped; use quotes if a name contains a comma).
- **Articles:** Admin > Insights. Separate paragraphs with a blank line. Plain text only; formatting characters are shown as typed, never executed.
- **Back up everything:** copy `data.db` (and use Export CSV for enquiries).

## API
Public: `POST /api/contact`, `GET /api/clients`, `GET /api/posts`, `POST /api/track`, `GET /api/health`.
Admin (header `Authorization: Bearer <ADMIN_TOKEN>`): `/api/admin/enquiries[.csv]`, `/api/admin/clients[/import]`, `/api/admin/posts`, `/api/admin/stats`.

## Data collected and privacy
- Enquiries store the name, company, email, phone, service, message and IP address (for abuse control). Mention this in your privacy policy.
- Page views are counted per page per day with no cookies, no IP and no personal data.

## Deploy
Any Node 18+ host (Render, Railway, Fly.io, VPS) with HTTPS, the variables from `.env.example`, and a **persistent disk** for `DB_PATH`. Keep `ADMIN_TOKEN` long and secret.

## Not included (ask if you need them)
Image uploads, multiple admin users/roles, and editing the fixed page copy (services, leadership) from the admin panel.
