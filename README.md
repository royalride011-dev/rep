# Royal Ride Jordan

Royal Ride Jordan is a React/Vite one-page luxury transportation website for private chauffeur services, airport transfers, executive travel, private tours, and destination transportation across Jordan.

The project is an existing pnpm workspace. The Royal Ride website is the `@workspace/royal-ride-clone` web artifact and keeps its current Royal Ride visual identity, photography, typography, navigation, quote flow, and WhatsApp contact paths.

## Tech stack

- React 19 with TypeScript
- Vite 7
- pnpm workspaces
- Node.js 24 in the Replit workspace
- Vercel static deployment for the Royal Ride web artifact
- Express/Drizzle/PostgreSQL packages are present for the separate optional API workspace

## Repository map

```text
artifacts/royal-ride-clone/  React/Vite Royal Ride website
artifacts/api-server/        Separate Express API artifact
lib/api-spec/                OpenAPI source
lib/api-client-react/        Generated API client
lib/api-zod/                 Generated API validation types
lib/db/                      Drizzle/PostgreSQL package
vercel.json                  Vercel build and SPA routing configuration
```

The Royal Ride website is currently a static client-side application. Its quote form prepares a WhatsApp inquiry; it does not submit booking data to the separate API or database.

## Requirements

- Node.js 22 or newer (Node.js 24 is used by the Replit workspace)
- pnpm 10

Check the local versions:

```bash
node --version
pnpm --version
```

## Install

From the repository root:

```bash
pnpm install --frozen-lockfile
```

Do not use `npm install` or `yarn install`; the workspace lockfile and package lifecycle enforce pnpm.

## Local development

Start the Royal Ride website:

```bash
pnpm dev
```

The site runs on port `19432` by default. To use a different port or mount path:

```bash
PORT=3000 BASE_PATH=/ pnpm dev
```

The artifact workflow is also available through the Replit workspace and uses the same web package.

## Validation and production build

Run the full workspace typecheck:

```bash
pnpm run typecheck
```

Build the complete workspace:

```bash
pnpm run build
```

Build only the Royal Ride website:

```bash
pnpm --filter @workspace/royal-ride-clone run build
```

Preview the built Royal Ride site locally:

```bash
pnpm preview
```

The website output is written to `artifacts/royal-ride-clone/dist/public`.

`PORT` and `BASE_PATH` are optional for local builds. Defaults are `19432` and `/`; deployment platforms can override them.

## Environment variables

### Royal Ride web app

The web app has no required private secrets.

| Variable | Required | Default | Purpose |
| --- | --- | --- | --- |
| `PORT` | No | `19432` | Vite development/preview server port |
| `BASE_PATH` | No | `/` | Vite base path for generated asset URLs |

The WhatsApp number is a public click-to-chat destination, not an authentication secret. The optional analytics adapter is defensive and only uses an existing `window.umami` object when one is provided by the host.

### Separate API/DB workspace

The optional API and database packages require:

| Variable | Required | Purpose |
| --- | --- | --- |
| `DATABASE_URL` | When running the API/DB packages | PostgreSQL connection string |
| `PORT` | When running the API server directly | Express server port |
| `NODE_ENV` | Recommended | Runtime environment |
| `LOG_LEVEL` | Optional | API logger level |

Never commit real values. Use Replit Secrets, Vercel Environment Variables, or a local ignored `.env` file.

## Vercel deployment

The repository includes `vercel.json` for the Royal Ride artifact. It configures:

- Vite framework detection
- frozen pnpm installation
- `pnpm --filter @workspace/royal-ride-clone run build`
- output directory `artifacts/royal-ride-clone/dist/public`
- `PORT=3000` and `BASE_PATH=/` during the Vercel build
- SPA fallback rewrites to `index.html`

To deploy:

1. Import the GitHub repository into Vercel.
2. Keep the repository root as the Vercel project root.
3. Confirm Vercel detects the checked-in `vercel.json`.
4. Set the production domain in Vercel.
5. Run a production deployment.
6. Verify `/`, `/robots.txt`, `/sitemap.xml`, the favicon, images, the quote modal, and WhatsApp links on the deployed URL.

No Vercel API token or account credential is stored in this repository. Vercel authorization and the first deployment must be completed by the account owner.

## Custom domain

The intended canonical host is:

```text
https://royalridejo.com
```

The site metadata, canonical URL, Open Graph URL, sitemap, and robots file use that host. Add both `royalridejo.com` and `www.royalridejo.com` in Vercel, then configure the non-www host as the primary domain so Vercel can redirect `www` to the canonical host.

For a standard Vercel DNS setup, the records are commonly:

```text
A      @      76.76.21.21
CNAME  www    cname.vercel-dns.com
```

DNS values can be project-specific or changed by Vercel. Use the exact records displayed in the Vercel Domains panel before editing the registrar. DNS changes are intentionally not automated by this repository.

## GitHub readiness

The repository is structured for GitHub synchronization and ignores:

- dependencies
- `.env` and local environment files
- build output and Vercel output
- coverage, logs, caches, and editor files
- uploaded source assets that are not required by the web build

Before synchronizing, review `git status`, confirm the intended branch, and resolve any divergence between the local branch and GitHub. Do not commit secrets or generated `dist` output.

## SEO and production behavior

The site includes:

- static title and meta description
- canonical URL
- Open Graph and Twitter card metadata
- favicon
- LocalBusiness and Service JSON-LD
- `robots.txt`
- `sitemap.xml`
- descriptive image alt text
- semantic page headings and section anchors

The app also includes a user-facing error boundary, quote form validation and success states, loading-safe lazy booking modal behavior, and a styled not-found component for future routed pages. The current product is intentionally a one-page site, so the primary production route is `/`.

## Troubleshooting

### Build says `PORT` or `BASE_PATH` is missing

Current defaults make these variables optional. If you need explicit values:

```bash
PORT=19432 BASE_PATH=/ pnpm --filter @workspace/royal-ride-clone run build
```

### Assets do not load under a subpath

Set `BASE_PATH` to the exact deployment path, including the trailing slash when required by the host. The app uses Vite's `BASE_URL` for runtime image paths.

### Vercel returns a 404 after refreshing a client-side URL

Keep the SPA rewrite in `vercel.json`. The Royal Ride product currently uses section anchors rather than separate client-side routes.

### API development fails

The API is a separate workspace package and requires a provisioned PostgreSQL connection:

```bash
DATABASE_URL="..." PORT=8080 pnpm --filter @workspace/api-server run dev
```

Keep the connection string out of shell history when possible and never commit it.