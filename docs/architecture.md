# Architecture notes

## Multi-tenancy

Every club's data is isolated at the database layer. Access is enforced with Postgres row level security rather than in application code, so a bug in the UI cannot expose another club's players or selections.

## Client

A single-page React app, built with Vite and shipped as an installable PWA so coaches can use it pitchside on a phone. Server state is fetched through small per-feature hooks (players, sessions, drills, selection board and so on) that wrap the Supabase client. Heavy routes are lazy loaded.

## Player-facing surface

Players use a separate, lightweight hub with its own shell and sign-in flow. It is deliberately smaller than the coach app: fixtures, availability and selection status only.

## Drag-and-drop selection

Team selection and depth ordering use dnd-kit. The selection board and depth chart share the same underlying player pool, so a change in one is reflected in the other.

## Documents

Team sheets and drill sheets render to PDF in the browser, so coaches can print or share them without a server round trip.

## Delivery

Work is specified phase by phase before it is built. Pull requests run a TypeScript check and the test suite in GitHub Actions. The database evolves through versioned SQL migrations.
