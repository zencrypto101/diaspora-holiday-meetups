# Diaspora Holiday Meetups

Fixed meetup spots for immigrants, students and expats on their national holidays. "Time is set by the holiday. We just tell you the place."

## Stack
- Vite 8 + React 19 + TypeScript 7 (strict)
- Map: Mapbox GL JS (primary, requires `VITE_MAPBOX_TOKEN`) with Leaflet + OpenStreetMap fallback (no key)
- Holidays: Nager.Date (200+ countries) + bundled static JSON for the 11 countries Nager.Date does not cover
  (IN, TH, MY, PK, LK, NP, MM, MU, RE, YT, OM) — see `src/data/holidays/README.md`

## Run
```
pnpm install
pnpm dev
```

## Build & verify
```
pnpm run build                   # tsc --noEmit && vite build
pnpm run test                    # vitest
npx eslint .                     # lint (react-hooks); types/unused are enforced by tsc
node scripts/runtime-proof.cjs   # headless-Chrome golden-path assertions + screenshot
node scripts/smoke-test.cjs      # tsc + eslint + build + test + runtime proof
node scripts/verify-holiday-matching.cjs   # which spots actually match holiday data
node scripts/verify-holiday-matching.cjs --strict   # gate: fails on an unexplained mismatch
                                       # (part of smoke-test; offline is reported as
                                       #  INCONCLUSIVE, not treated as a mismatch)
```

## Map token (optional)
Create a free Mapbox account, generate a public token, set `VITE_MAPBOX_TOKEN` in `.env`.
Without it the app uses the Leaflet + OSM fallback and the Mapbox branch stays unexercised
(see `reports/mvp-verification.md` "Honest limits").

## Data
- Seed spots: `output/GATHERINGS_SEED.csv` (Researchy) → regenerated into `src/data/spots.ts`
  via `node scripts/csv-to-spots.cjs`. Never hand-edit `spots.ts`; never hardcode rows in the
  generator — a row that is not in the CSV does not exist.
- Current corpus: 33 spots, 9 verified. Verified/unverified flags are preserved; the
  cancelled 2014 Singapore event is excluded.

## Status
Deployment is blocked pending the Reviewer's DHM-15 re-audit. See
`reports/mvp-verification.md` for exactly what is and is not verified.
