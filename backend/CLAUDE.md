# Server — Fastify / Node.js Conventions

Applies to `server/src/**` — Fastify routes, server middleware, and the outbreaks-fetcher proxy logic.

## Module System

- The server is **pure ESM** (`"type": "module"` in `package.json`).
- All local imports must use the `.js` extension (TypeScript `moduleResolution: NodeNext`):
  ```ts
  import { outbreakRoutes } from './routes/outbreaks.js'
  ```

## Route Registration

- Group routes in `server/src/routes/` as named `FastifyPluginAsync` functions and register them via `fastify.register(...)` in `app.ts`.
- Use Fastify's typed request/reply with inline schema objects when adding new endpoints.

## Upstream Proxy

- All fetcher calls go through `FETCHER_API_BASE_URL` (default `http://localhost:8000`).
- Read it once at startup: `const BASE = process.env.FETCHER_API_BASE_URL ?? 'http://localhost:8000'`.
- Use the built-in `fetch` (Node 18+) — no `axios` or `node-fetch`.
- Always forward pagination params (`page`, `limit`) when proxying list endpoints.

## Runtime Config

- `routes/config.ts` serves `GET /api/map-config`, which hands the browser the CARTO basemap key. It exists because the key must **not** be a `VITE_` build variable: the Docker image is published to a public registry, so a build-time key would ship inside it.
- `CARTO_API_KEY_FILE` (path to a mounted secret) takes precedence over `CARTO_API_KEY` (plain value); both are optional, and neither set means CARTO serves watermarked tiles.
- The value is trimmed, and an unreadable secret file logs and degrades to an empty key rather than failing to boot — a missing basemap must never take the API down.
- Resolved once at plugin registration, like `FETCHER_API_BASE_URL`, so a rotated secret file needs a restart.

## Telemetry

- Both telemetry env vars are optional; unset (the default) means no instrumentation is installed.
- `OTEL_EXPORTER_OTLP_ENDPOINT` (gRPC, e.g. `http://otel-lgtm:4317`) — the server's own traces/metrics/logs. `setupTelemetry()` must be imported before `fastify` in `index.ts` so the auto-instrumentation patches land first.
- `OTEL_EXPORTER_OTLP_HTTP_ENDPOINT` (HTTP, e.g. `http://otel-lgtm:4318`) — enables `routes/otel.ts`, which relays the browser's OTLP payloads same-origin because the collector has no published port. It also drives `GET /otel/config`, which the client checks before instrumenting.

## Response Shaping

Client-expected response shapes are fixed — do not rename top-level fields. Key routes:

| Route | Returns |


## Error Handling

- Log errors with `fastify.log.error` (structured JSON logger).
- Return `{ error: 'message' }` with an appropriate 4xx/5xx status code.
- Never surface internal stack traces to the client.
