# YC Directory

A Next.js 14 app that showcases and explores startups, powered by Sanity CMS. Browse startup cards, view detailed pitches rendered from Markdown, filter by category, and discover editor-curated picks. Sentry is integrated for error monitoring.

## Features

- **Startup listing**: Card grid with author, views, date, and category
- **Detail page**: Markdown-rendered pitch, author profile, hero image
- **Search/filtering**: GROQ-powered query on title, category, and author
- **Editor picks**: Curated playlist section via Sanity
- **Views tracking**: Suspense-wrapped view component for counting
- **Responsive UI**: Shadcn UI, Tailwind, Lucide icons
- **Error monitoring**: Sentry source maps and instrumentation
- **Optimized images**: Next/Image with remote patterns

## Tech Stack

- **Frontend**: Next.js App Router, TypeScript, Tailwind, Shadcn UI
- **CMS**: Sanity (GROQ queries)
- **Monitoring**: Sentry for Next.js
- **Icons**: Lucide-react
- **Markdown**: markdown-it

## Getting Started

### Prerequisites
- Node.js 18+
- A Sanity project (Project ID and dataset)
- Optional: Sentry project for DSN and source maps

### 1) Install dependencies
```bash
npm install
# or
yarn
# or
pnpm i
# or
bun i
```

### 2) Environment variables
Create a `.env.local` in the project root and add:

```bash
# Sanity
NEXT_PUBLIC_SANITY_PROJECT_ID=your_project_id
NEXT_PUBLIC_SANITY_DATASET=production
# If you use authenticated client operations, also add:
# SANITY_API_TOKEN=your_sanity_token

# Sentry (optional but recommended)
SENTRY_DSN=your_sentry_dsn
```

Make sure your Sanity CORS settings allow requests from your dev and production domains.

### 3) Run the development server
```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open `http://localhost:3000`.

## Scripts

```bash
npm run dev      # Start dev server
npm run build    # Build for production (uploads Sentry source maps if configured)
npm run start    # Start production server
```

## Project Structure (high level)

app/
  (root)/
    startup/[id]/page.tsx   # Startup details page with editor picks + views
components/
  StartupCard.tsx           # Startup card UI
  ui/                       # Shadcn primitives (button, skeleton, toast, etc.)
sanity/
  lib/queries.ts            # GROQ queries for startups, authors, playlists
  schemaTypes/              # Sanity schemas (startup, author, playlist)
next.config.ts              # Sentry + images config
instrumentation-client.ts   # Client instrumentation
README.md

## Data Model (Sanity)

startup: title, slug, author, description, category, image, pitch (Markdown), views
author: name, username, image, bio
playlist: title, slug, select[] -> references to startups (used for “Editor Picks”)
See sanity/lib/queries.ts for GROQ queries:
STARTUPS_QUERY – list and search
STARTUP_BY_ID_QUERY – detail page data
PLAYLIST_BY_SLUG_QUERY – editor picks
Notable Implementation Details
Detail page (app/(root)/startup/[id]/page.tsx) fetches startup and editor picks in parallel, renders the pitch via markdown-it, and wraps view counting in Suspense.
Cards (components/StartupCard.tsx) display author, date, views, description, image, and category links.
Sentry is enabled via withSentryConfig in next.config.ts with productionBrowserSourceMaps: true.
Deployment
Deploy to Vercel or any Node-capable host.
Ensure environment variables are configured in your hosting platform.
If using Sentry, confirm SENTRY_DSN is set and source map upload is permitted in CI.

## License
MIT


