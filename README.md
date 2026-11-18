# TechSmart — Company Website with Database

TechSmart is a company website for a software and IT services business, built
with TanStack Start (React 19 + TypeScript) and backed by a PostgreSQL database
(Supabase). Visitors browse the company pages and send an inquiry through a
contact form; the inquiries and all page texts are stored in the database, and
an administrator edits them from a protected panel.

## Pages

| URL | Content |
| --- | --- |
| `/` | Home: hero, services preview, reasons, latest news |
| `/services` | The three services with features and pricing notes |
| `/about` | Vision, mission, and goals |
| `/partners` | Partner organisations |
| `/news`, `/news/:id` | Article list and article detail |
| `/contact` | Inquiry form that writes to the database |
| `/privacy` | Privacy notice |
| `/auth`, `/reset-password` | Sign in, register, password reset |
| `/admin` | Administration panel (sign-in + admin role required) |

## Technology

- **Framework:** TanStack Start v1 (React 19, TypeScript, Vite) — file-based routing under `src/routes`
- **Styling:** Tailwind CSS v4, design tokens in `src/styles.css` (blue and white identity)
- **Database & auth:** Supabase (PostgreSQL, Row Level Security, email/password auth)
- **Server calls:** TanStack server functions (`createServerFn`) in `src/lib`
- **Tests:** Vitest + Testing Library in `src/test`

## Project structure

```
src/
  routes/          one file per URL, including /admin behind _authenticated
  components/      shared site layout and cards (site.tsx) + UI primitives (ui/)
  lib/             content queries, server functions, access rules
  integrations/    Supabase client and generated types
  assets/          images bundled with the site
  test/            unit tests
database/
  techsmart_database.sql   full schema + seed data in one readable file
  README.md                table-by-table documentation
drizzle/migrations/        the migrations as they were applied
```

## Getting started

You need Node.js 20+ and Bun (or npm).

```sh
bun install        # or: npm install
cp .env.example .env   # then fill in your own project values
bun run dev        # or: npm run dev
```

Open http://localhost:8080.

## Database

The schema, grants, security policies, and the content the site starts with are
in [`database/techsmart_database.sql`](database/techsmart_database.sql), with a
plain-language description of every table in
[`database/README.md`](database/README.md).

Summary of the access rules:

- **Visitors** may read published content and submit one contact message.
- **Signed-in users** see the same public content plus their own role row.
- **Administrators** additionally edit content and read messages. The role lives
  in its own table (`user_roles`) and is checked by a security-definer function,
  never by client-side storage.

## Administrator access

The owner's account receives the `admin` role automatically once its email
address is confirmed; the email is set in the `assign_designated_admin` trigger
function. To change who the administrator is, edit that function (or add a row
to `user_roles` from a privileged session).

## Tests

```sh
bun run test       # or: npm run test
```

## Hosting

The app is written for TanStack Start and can be deployed to any host that runs
it (Cloudflare Workers is the default build target). Routing needs no extra
redirect rules.
