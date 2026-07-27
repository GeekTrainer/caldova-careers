# Caldova Careers

The careers site for **Caldova**, an AI-native pharmaceutical company. Browse open roles and submit an application with your name and email — no account required.

This is the sample application for the **GitHub Copilot CLI** workshop. It's a deliberately small, realistic app: job postings are Markdown content, and the only database-backed feature is the apply flow.

> [!NOTE]
> Caldova is a fictional company created for demonstration and training purposes. The roles, locations, and applications in this app are not real.

## Tech stack

- **[Astro](https://astro.build/)** — static output by default, with a single on-demand route for the apply endpoint (via the `@astrojs/node` adapter)
- **[Tailwind CSS v4](https://tailwindcss.com/)** — styling, themed with the Caldova brand palette
- **Astro Content Collections** — job postings are Markdown + frontmatter in `src/content/jobs/` (validated by a Zod schema)
- **[Drizzle ORM](https://orm.drizzle.team/) + libSQL (SQLite)** — the `applications` table only
- **[Vitest](https://vitest.dev/)** — unit tests for pure logic and an integration test for the apply insert
- **[Playwright](https://playwright.dev/)** — end-to-end tests (runs against the built Node server)

## Getting started

```bash
npm install
npm run dev
```

`predev` runs `db:setup` (applies the `applications` migration) before starting the dev server at <http://localhost:4321>.

## Data model

- **Job postings** are files, not database rows: one Markdown file per role in `src/content/jobs/`, with frontmatter (`title`, `department`, `location`, `type`, `remote`, `postedDate`, `summary`) and a Markdown body. The schema lives in `src/content.config.ts`.
- **Applications** are the only relational data: the `applications` table (`db/schema.ts`) stores each submission — a logical `jobId` reference, the applicant's `name` and `email`, and optional `note` / `links`. There is no authentication; the applicant supplies their details directly on the form.

## Project structure

- `src/content/jobs/` — job postings (Markdown + frontmatter)
- `src/content.config.ts` — content collection schema
- `src/lib/jobs.ts` — pure helpers over `Job[]` (sorting, formatting)
- `src/lib/applications.ts` — application validation schema + insert helper
- `src/lib/db.ts` — Drizzle/libSQL client for the `applications` table
- `src/pages/` — `index.astro` (listing), `roles/[slug].astro` (detail + apply form), `api/apply.ts` (on-demand endpoint), `thanks.astro`, `about.astro`, `404.astro`
- `src/components/` — reusable `.astro` components
- `db/` — Drizzle schema, migrations, and migrate/test helpers
- `e2e-tests/` — Playwright tests

## Scripts

- `npm run dev` — start the dev server (`predev` migrates the database)
- `npm run build` — build the site (`prebuild` migrates the database)
- `npm run start` — run the built Node server (`dist/server/entry.mjs`)
- `npm run test:unit` — Vitest unit + integration tests
- `npm run test:e2e` — Playwright e2e tests (builds + starts the server first)
- `npm run lint` — ESLint
- `npm run typecheck` — `astro check`
- `npm run db:generate` / `db:migrate` / `db:setup` — Drizzle schema/migration tasks
