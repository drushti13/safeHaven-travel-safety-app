# Workspace

## Overview

pnpm workspace monorepo using TypeScript. TravelShield — a Mumbai travel safety mobile app built with Expo.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM (not yet used — using AsyncStorage)
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Mobile**: Expo SDK 54, Expo Router 6, react-native-maps 1.18.0
- **AI**: Google Gemini via Replit AI Integrations (proxied through API server)

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (Gemini proxy routes)
│   └── mobile/             # Expo React Native app (TravelShield)
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## TravelShield Mobile App Features

1. **QuickShield SOS** — Hold-to-activate SOS button, emergency contacts management, safe words
2. **SafeRoute Navigator** — AI-powered safe route via Gemini, Mumbai danger zone map
3. **SafeSpot Finder** — Police stations, hospitals on map, nearest spots by location
4. **Trip Tracker** — Trusted companions, check-in intervals, trip management
5. **Community Alerts** — Real-time incident reporting with map view

## Mumbai Dataset

Built-in `constants/mumbaiData.ts` with pincodes, danger indexes (0-10), and safe spots (police, hospital, fire, public, shelter).

## API Routes (api-server)

- `POST /api/safety/safe-route` — Gemini-powered route analysis
- `POST /api/safety/area-tip` — Gemini area safety tip
- `GET /api/healthz` — Health check

## Key Files (Mobile)

- `context/AppContext.tsx` — Global state (contacts, trips, alerts, SOS status)
- `constants/mumbaiData.ts` — Mumbai safety dataset
- `constants/colors.ts` — Design tokens (navy + red theme)
- `services/gemini.ts` — API calls to backend Gemini proxy
- `components/SOSButton.tsx` — Animated hold-to-activate SOS button
- `stubs/react-native-maps.web.js` — Web stub for react-native-maps
- `metro.config.js` — Web alias for react-native-maps

## Environment Variables

- `AI_INTEGRATIONS_GEMINI_BASE_URL` — Gemini API base URL (auto-provisioned)
- `AI_INTEGRATIONS_GEMINI_API_KEY` — Gemini API key (auto-provisioned)
- `SESSION_SECRET` — Session secret

## Notes

- react-native-maps pinned to 1.18.0 (Expo Go compatible)
- Gemini calls proxied through Express API server (env vars not available client-side)
- All data persisted in AsyncStorage (no database yet)
