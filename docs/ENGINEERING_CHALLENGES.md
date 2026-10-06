# Engineering Challenges

## Cross-platform appliance specs

Different appliance types have incompatible specs: a washing machine's load capacity and spin speed do not apply to a refrigerator. Using a single "specs" JSONB blob risked runtime type errors and complex client-side validation. The solution was type-specific Postgres tables (fridge_specs, washing_machine_specs, ac_specs) with tailored columns, so queries and comparison logic could safely assume the right fields existed. The trade-off: three separate joins instead of one generic lookup, but correctness and clarity won.

## Avoiding duplicate chat threads

Without a guard, a buyer could open several conversations with the same seller for a single listing, fragmenting the discussion. The app checks whether a thread for that (listing, user pair) already exists before creating a new one, and reuses it if so.

## Image handling in demo mode

The original app uploads appliance and listing photos to Supabase Storage, requiring live credentials and network access. For the portfolio demo, a small local asset server serves generated illustrations; the image service switches between local and cloud URLs via an environment flag. This let us showcase the photo gallery and listing workflow without exposing secrets or requiring a Supabase account.

## Real-time reviews with Supabase Realtime

The app subscribes to a Supabase Realtime channel for reviews. The engineering concern is subscription lifecycle: channels should be cleaned up when a screen is left, otherwise listeners accumulate.

## Mocking Supabase for offline demo

The app calls Supabase for every data and auth operation. To run a full demo without live services, we built a minimal in-memory mock (`demoSupabase.js`) that satisfies the client interface: auth methods, query builders, storage operations, and realtime subscriptions. It returns seeded data from `demoData.js` and simulates async behavior. The mock is swapped in at the client entry point via an environment flag, so the same service layer code works with real or mocked Supabase.

## Multi-screen state consistency

With 30 routes and several edit flows (listing creation, profile updates, review posting), state has to stay consistent across navigation. Auth state lives in a single React Context fed by Supabase's auth-change listener; catalogue, listing and chat data are fetched through the service layer.

## ML model portability

The ML training script runs offline using synthetically generated data. For the demo to work without Python tooling in the portfolio, the trained model is serialized to a binary pickle file and loaded by the FastAPI server at startup. This traded model transparency for portability: the pickle is opaque, but requires no retraining in the demo.
