# Raad Dashboard

This Next.js app is deployed to Cloudflare Workers through the OpenNext Cloudflare adapter. The Cloudflare project root is `dashboard_test/`; the static website in the repository root is a separate project.

## Local commands

Run `npm ci` to install the locked dependencies, `npm run dev` for local Next.js development, and `npm run preview` to build and preview the app in the Workers runtime.

## Cloudflare Workers Builds

Connect the GitHub repository in Cloudflare Workers Builds and configure:

- Root directory: `dashboard_test`
- Build command: `npx @opennextjs/cloudflare build`
- Deploy command: `npx @opennextjs/cloudflare deploy`
- Build environment: `NODE_VERSION=24`
- Additional build variables: none

The OpenNext build command runs the `build` script (`next build`) and then generates the Cloudflare Worker output. The deploy command publishes that generated Worker. For a gradual deployment, use `npx @opennextjs/cloudflare upload` as the deploy command instead.

The matching local commands from this directory are `npm run preview` and `npm run deploy`. The Worker entry point and asset directory are configured in `wrangler.jsonc`. `WORKER_SELF_REFERENCE` points back to the same Worker. No R2 cache, image service, database, or other binding is currently needed by this starter.

## Environment variables and secrets

The application currently reads no environment variables and requires no secrets. Do not create production variables until an app feature needs them. Add any future server credentials as Worker secrets, never as `NEXT_PUBLIC_*` values.

## Runtime notes

The app currently has one static App Router page, no API routes, dynamic routes, middleware, authentication, or database. Local images are served without runtime image optimization. Geist is fetched by `next/font` during the build and embedded in the generated assets; builds therefore need npm registry access as well as network access for the font fetch.

The `robots.txt`, `sitemap.xml`, and `CNAME` in the repository root belong to the separate static site. Set a real production hostname before adding canonical URLs, Open Graph URL metadata, or a sitemap for this app.
