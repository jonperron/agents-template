# Client — React / TypeScript Conventions

Applies to `client/src/**` — React components, hooks, RTK Query endpoints, and client-side logic.

## Component Structure

- Functional components only; no class components.
- Co-locate the CSS Module next to the component: `Foo.tsx` + `Foo.module.css`.
- Export the component as the **default export**.
- Keep prop interfaces defined in the same file; name them `Props` (not `FooProps`).

## Styling

- **CSS Modules exclusively** — import as `import styles from './Foo.module.css'`.
- No inline `style={}` props unless unavoidable (e.g., dynamic values computed at runtime).
- No Tailwind, no styled-components.

## State & Data Fetching

- Use **RTK Query** hooks from `outbreaksApi` for all server data (`useGetOutbreaksQuery`, etc.).
- No local `useState` for data that belongs to the server — keep the RTK Query cache as the source of truth.
- Local UI state (open/closed modals, hover, etc.) lives in `useState` inside the component that owns it.

## Translations

- Every user-visible string must use `const { t } = useTranslation()`.
- Never hardcode English (or French) strings in JSX — use a translation key.
- Add keys to **both** `en.json` and `fr.json` in `client/src/i18n/locales/`.

## Map Layer

- Map rendering uses **React-Leaflet v5** (`MapContainer`, `TileLayer`, `Marker`, `Popup`).
- Get the raw Leaflet map instance via the `MapCapture` helper in `OutbreakMap.tsx`.
- Clustering logic lives in `MapMarkers.tsx` — prefer extending it over creating parallel marker logic.

## Filter Chain

When adding a new filter dimension, slot it into the existing chain in `OutbreakMap.tsx`:
**time → source → affectedPopulation → category → disease**

Propagate the new filter state down through `FilterModal` as new props; follow the existing `onXToggle / onXSelectAll / onXSelectNone` callback pattern.

## TypeScript

- Strict mode is on; avoid `any`.
- Shared domain types (`Outbreak`, `Alert`, `LocationGroup`) are in `client/src/types.ts` — extend there if the shape changes.
- API response types (stats, breakdowns) are in `outbreaksApi.ts`.
