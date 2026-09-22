# Royal Ride Jordan workspace

This pnpm workspace contains the Royal Ride Jordan one-page luxury transportation website plus a separate optional API/DB stack.

## Run & Operate

- `pnpm dev` — run the Royal Ride website on port 19432
- `pnpm preview` — preview the built Royal Ride website
- `pnpm --filter @workspace/api-server run dev` — run the separate API server
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Royal Ride web defaults: `PORT=19432`, `BASE_PATH=/`
- API/DB env: `DATABASE_URL`, with `PORT` and `NODE_ENV` when running the API directly

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Website: React + Vite
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/royal-ride-clone/` — Royal Ride website source, public assets, and Vite config
- `artifacts/royal-ride-clone/.replit-artifact/artifact.toml` — Replit artifact workflow and static deployment settings
- `artifacts/api-server/` — separate API artifact
- `lib/api-spec/openapi.yaml` — API contract source of truth
- `lib/db/src/schema/` — database schema source of truth
- `vercel.json` — Vercel build, output, and SPA fallback configuration
- `README.md` — local development, deployment, domain, and troubleshooting guide

## Architecture decisions

- The Royal Ride website is intentionally a static one-page React/Vite app.
- Quote requests are prepared for WhatsApp; the web artifact does not currently write booking data to the API or database.
- Vite defaults `PORT` and `BASE_PATH` so local builds do not depend on Replit-only environment injection.
- The production website is served as static output with an SPA fallback; section anchors are used instead of invented service routes.

## Product

Royal Ride presents luxury transportation services in Jordan, including airport transfers, private chauffeur service, executive travel, private tours, fleet options, destination journeys, and a WhatsApp-assisted private quote flow.

## User preferences

- Preserve the existing Royal Ride colors, typography, logo, imagery, layout, and luxury visual language.
- Do not invent company claims, reviews, awards, statistics, or service pages.

## Gotchas

- Use pnpm, not npm or yarn.
- Run the Royal Ride build with `pnpm --filter @workspace/royal-ride-clone run build`.
- Keep real `DATABASE_URL` values in secrets or ignored local files.
- The Replit API and Royal Ride web artifacts are separate services.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
