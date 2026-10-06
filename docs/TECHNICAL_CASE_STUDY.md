# Technical Case Study

## Stack choice

**React Native with Expo.** The app targets iOS and Android with a shared codebase. Expo's web target (react-native-web) made it possible to run a local demo without a native build environment. File-based routing via expo-router keeps navigation simple at 30 screens.

**Supabase as backend.** Postgres, auth, storage and realtime channels replaced multiple third-party services. Direct client access via `@supabase/supabase-js` avoided building a separate API server for catalogue and marketplace operations.

**FastAPI for ML.** A lightweight Python API served price predictions independently, keeping ML training and serving separate from mobile and database concerns.

## Key decisions

**Multi-table appliance specs.** Different appliance types (fridges, washing machines, ACs) have distinct properties. Rather than a single generic "specs" JSONB column, type-specific tables (fridge_specs, washing_machine_specs, ac_specs) with columns tailored to each type allowed SQL joins for comparison queries and spec highlighting.

**Service layer abstraction.** All Supabase and API calls flow through a thin service layer (appliance, brands, chat, selling, image upload services). This let us swap a real Supabase client for an in-memory mock in demo mode without duplicating business logic.

**Conversation deduplication.** Chat threads tie to (listing, seller, buyer). The app checks for an existing conversation before creating a new one, so repeat chats with the same seller do not spawn duplicate threads.

**Demo data seeding.** A fictional user base, appliances, listings, reviews and chat history in `demoData.js` and a stand-in `demoSupabase.js` client let the portfolio run entirely offline, showing the full user journey without exposing credentials or requiring external services.

## Engineering highlights

- **End-to-end ML integration.** The prediction pipeline flows from form input through service layer to FastAPI and back as a single feature, with client-side validation of the form before the request is sent.
- **Image upload.** Photos for listings upload to Supabase Storage through a dedicated image service, which also resolves public URLs.
- **Realtime review channel.** The app subscribes to a Supabase Realtime channel for reviews.
- **Auth state via Context.** Supabase's `onAuthStateChange` listener syncs auth to React Context, driving screen transitions on login and logout.

## Trade-offs

**Synthetically trained model.** The ML model learns a hand-written pricing formula from seeded data, not market data. This is acceptable for a portfolio: it demonstrates the architecture and ML integration; production would require real training data.

**Schema mismatch.** The app uses camelCase column names in code (modelNumber, Price, userId) that diverge from the database schema's snake_case. The portfolio schema snapshot (`database_schema.sql`) is out of sync. A refactor would align both to one convention.

**Hard-coded service URLs.** Supabase and FastAPI endpoints are hard-coded in the original app. Moving them to environment configuration is the obvious next step.
