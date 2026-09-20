# PhonePilot AI

Native Kotlin + Jetpack Compose Android assistant for safe, user-confirmed actions on the user's own phone.

## Run & Operate

- Open `android/` in Android Studio and run the `app` configuration on an Android device or emulator.
- `cd android && ./gradlew assembleDebug` — build the debug APK when Android SDK Platform 35 is installed.
- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)
- Android: Kotlin, Jetpack Compose, Android Gradle Plugin, standard Android APIs

## Where things live

- `android/` — the standalone Android Studio project for PhonePilot AI.
- `android/app/src/main/java/com/phonepilot/ai/engine` — local command parser.
- `android/app/src/main/java/com/phonepilot/ai/actions` — modular phone action implementations.
- `android/app/src/main/java/com/phonepilot/ai/MainActivity.kt` — Compose UI and permission/voice launchers.
- `android/README.md` — device setup, commands, permissions, and Android limitations.

## Architecture decisions

- V1 uses a deterministic local parser; `PhoneAction` and `ActionRegistry` are the extension point for a future AI model.
- Calls and alarms are modeled as confirmation-required results, not direct side effects.
- History is stored locally with SharedPreferences; no server or API key is needed.
- Screenshot requests explicitly explain Android's silent-screenshot restriction instead of faking success.

## Product

The Android app accepts voice or text commands for calls, app launch, flashlight,
media volume, alarm setup, screenshot guidance, and browser search. It also provides
permission explanations, safety settings, and persisted command history.

## User preferences

- Keep PhonePilot AI focused on V1 safe local phone actions; do not add hidden surveillance, accessibility abuse, or security bypasses.

## Gotchas

- The Android module needs a local Android SDK with Platform 35; the current development container has Java and Gradle but no Android SDK.
- Do not add a backend dependency for V1 unless a future requirement explicitly needs one.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
