# Repository Guidelines

This repository contains a PostgreSQL 18 setup and a SQL schema for an interview‑preparation system. Follow the guidance below to work consistently and safely.

## Project Structure & Module Organization
- `scheme.sql` — canonical database schema (tables, constraints, indexes).
- `compose.yaml` — Docker Compose service `db` (Postgres 18, port `5432`).
- `data/` — persisted database files (managed by Docker; do not edit manually).
- `README.md` — project overview and assignment details.

## Build, Test, and Development Commands
- `docker compose up -d` — start PostgreSQL locally.
- `docker compose ps` — verify the container is running.
- Inspect objects quickly:
  - `docker compose exec -T db psql -U db -d db -c "\\dn; \\dt+; \\du"`
- Reset environment (removes data volume):
  - `docker compose down -v`

## Coding Style & Naming Conventions
- SQL style: uppercase keywords, 2‑space indentation, one clause per line.
- Identifiers: `snake_case`.
- Tables: plural nouns (e.g., `courses`, `modules`, `files`, `user_experience`).
- Columns: `snake_case`; foreign keys as `<table>_id`.
- Primary keys: `id` (bigint/identity). Constraint/index names: `pk_<table>`, `fk_<table>_<ref>`, `idx_<table>_<cols>`, `uq_<table>_<cols>`, `chk_<table>_<rule>`.
- If adding incremental changes, create `migrations/<NNN>_<short_description>.sql`; keep `scheme.sql` in sync.

## Testing Guidelines
- Smoke checks: run `SELECT 1;`, list tables (`\\dt`), and validate DDL applies cleanly.
- Use `BEGIN; … ROLLBACK;` for exploratory changes. For performance, prefer `EXPLAIN (ANALYZE, BUFFERS)` on key queries.
- Add seed/test data in dedicated scripts under `migrations/` rather than editing `scheme.sql`.

## Commit & Pull Request Guidelines
- Use Conventional Commits (e.g., `feat:`, `fix:`, `docs:`, `chore:`) as seen in history.
- PRs must include: clear description, rationale, DDL summary (tables/columns/constraints affected), and any follow‑up tasks.
- Keep changes atomic (schema change + migration + docs). Link issues when applicable.

## Security & Configuration Tips
- Default credentials in `compose.yaml` are `db/db/db` for local use only; prefer overriding via a local `.env` (do not commit secrets).
- Treat `data/` as ephemeral; never edit files inside. Use `docker compose down -v` to reset the database state.

