# Privacy and Source

## Production source

The production source code is not included in this repository. All code, configuration and data structures remain private. This portfolio includes only the structural and architectural documentation necessary to evaluate the system.

## Demo data

This demo build runs on seeded fictional data to simulate user journeys without requiring live cloud services. All screenshots and demo flows use completely fictional users, appliances, listings, reviews and chat messages. No real user data, pricing, or personal information is included.

## Secrets and configuration

Sensitive configuration (project URLs, keys and API endpoint details) is excluded from this repository.

## Image assets

Appliance and listing photos in the demo use generated illustrations and placeholder images. No real product photography or proprietary brand assets are included in the portfolio.

## Third-party libraries and licenses

The app depends on open-source packages: React Native (MIT), Expo (MIT), Supabase SDK (Apache 2.0), FastAPI (MIT), scikit-learn (BSD), pandas (BSD), and numerous React / React Native packages. These remain subject to their respective licenses; no modification or reuse of proprietary third-party assets is implied.

## Access model

The app talks to Supabase directly from the client with the signed-in user's session; chat conversations are tied to a listing and its two participants. Database access rules are not part of this repository.
