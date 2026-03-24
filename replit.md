# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Frontend**: React + Vite + Tailwind CSS + Framer Motion

## Structure

```text
artifacts-monorepo/
├── artifacts/              # Deployable applications
│   ├── api-server/         # Express API server
│   └── audio-medics/       # Audio Medics ecommerce website (React + Vite)
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts (single workspace package)
│   └── src/                # Individual .ts scripts
├── pnpm-workspace.yaml     # pnpm workspace config
├── tsconfig.base.json      # Shared TS options
├── tsconfig.json           # Root TS project references
└── package.json            # Root package with hoisted devDeps
```

## Audio Medics Website (`artifacts/audio-medics`)

Premium ecommerce website for Audio Medics, a hearing care company in Lahore, Pakistan.

### Key Features
- Full product catalog with 43+ Unitron hearing aid products
- Product detail pages with image gallery, specs, features, and pricing in PKR
- Category filtering (BTE, RIC, Custom Made) and search
- Cart functionality with React Context
- Appointment booking form
- Contact form with WhatsApp CTA
- Services, About, Gallery, Events pages
- Sticky navbar with mobile drawer menu
- Sticky mobile CTA bar (Call/WhatsApp)
- Premium medical blue/teal/charcoal color palette
- Framer Motion animations

### Pages & Routes
- `/` — Homepage (hero, trust bar, featured products, services, testimonials)
- `/products` — Product listing with search and category filters
- `/products/:slug` — Product detail page
- `/services` — Services overview
- `/about` — About page with team image
- `/gallery` — Gallery with category filters
- `/events` — Events/workshops page
- `/contact` — Contact page with form
- `/book-appointment` — Appointment booking form
- `/cart` — Shopping cart

### Data Architecture
- Product data: `src/data/products.ts` (easy to update, JSON-like structure)
- Cart state: React Context (`src/context/CartContext.tsx`)
- Product images from `attached_assets/` via `@assets` alias

### Brand Details
- Company: Audio Medics
- Tagline: Better Hearing Starts Here
- Phone: +92 300 4344467
- Address: Plot 2&3-G Fatima Centre, 14-A Queen's Road, Lahore
- Facebook: https://web.facebook.com/audiomedicsHEARING/
- Instagram: https://www.instagram.com/audio_medics/

## TypeScript & Composite Projects

Every package extends `tsconfig.base.json` which sets `composite: true`. The root `tsconfig.json` lists all packages as project references.

- **Always typecheck from the root** — run `pnpm run typecheck`
- **`emitDeclarationOnly`** — only `.d.ts` files during typecheck
- **Project references** — when package A depends on B, A's tsconfig must list B in references

## Root Scripts

- `pnpm run build` — runs `typecheck` first, then recursively runs `build`
- `pnpm run typecheck` — runs `tsc --build --emitDeclarationOnly`

## Packages

### `artifacts/api-server` (`@workspace/api-server`)

Express 5 API server with routes at `/api`.

### `lib/db` (`@workspace/db`)

Database layer using Drizzle ORM with PostgreSQL.

### `lib/api-spec` (`@workspace/api-spec`)

OpenAPI 3.1 spec and Orval codegen config.

### `lib/api-zod` (`@workspace/api-zod`)

Generated Zod schemas from the OpenAPI spec.

### `lib/api-client-react` (`@workspace/api-client-react`)

Generated React Query hooks from the OpenAPI spec.

### `scripts` (`@workspace/scripts`)

Utility scripts package.
