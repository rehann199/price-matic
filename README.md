<div align="center">

# Price-Matic

**A mobile-first appliance marketplace: browse and compare new appliances, trade used ones with seller chat, and get an ML price estimate for an old appliance.**

React Native · Expo · Supabase · FastAPI · scikit-learn

![React Native](https://img.shields.io/badge/React%20Native-0.76-61DAFB?logo=react&logoColor=black)
![Expo](https://img.shields.io/badge/Expo-52-000020?logo=expo&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)

<table>
  <tr>
    <td><img src="assets/screenshots/price-matic-home.webp" width="250" alt="Home screen with brand grid and popular appliances" /></td>
    <td><img src="assets/screenshots/price-matic-compare.webp" width="250" alt="Side-by-side comparison of two refrigerators with a specification table" /></td>
    <td><img src="assets/screenshots/price-matic-valuation-result.webp" width="250" alt="Valuation result showing an estimated price and appliance summary" /></td>
  </tr>
</table>
<sub>Screenshots are real captures of a local build running on fictional demo data.</sub>

</div>

---

## Product at a Glance

| | |
|---|---|
| **What it is** | Appliance marketplace and price assistant for the Pakistani market (prices in PKR) |
| **Platform** | Mobile-first React Native / Expo app; the demo is captured from the Expo web target |
| **New appliances** | Brand catalogue, category filter, product pages with features and specifications |
| **Comparison** | Two appliances side by side, with all specs or key differences only |
| **Marketplace** | Used-appliance listings with filters, listing detail and seller chat |
| **Selling** | Listing form with photos and a "My Ads" screen with edit and delete |
| **Valuation** | Price estimate for a used appliance from a scikit-learn model served by FastAPI |
| **Reviews** | Per-appliance and global reviews, plus a write-review flow |
| **Admin** | Dashboard and inventory screens to add, edit and delete appliances |
| **Size** | 30 routes |

## Product Showcase

<table>
  <tr>
    <td align="center"><img src="assets/screenshots/price-matic-welcome.webp" width="190" alt="Welcome screen with logo and Getting Started button"/><br/><sub><b>Welcome</b><br/>Entry and login</sub></td>
    <td align="center"><img src="assets/screenshots/price-matic-brand-catalog.webp" width="190" alt="Brand catalogue with category filter chips and product cards"/><br/><sub><b>Brand catalogue</b><br/>Category filter</sub></td>
    <td align="center"><img src="assets/screenshots/price-matic-product-detail.webp" width="190" alt="Product page with gallery, rating, price and collapsible sections"/><br/><sub><b>Product page</b><br/>Gallery, specs, features</sub></td>
    <td align="center"><img src="assets/screenshots/price-matic-reviews.webp" width="190" alt="List of reviews with ratings"/><br/><sub><b>Reviews</b><br/>Ratings and comments</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="assets/screenshots/price-matic-marketplace.webp" width="190" alt="Used appliance listings with price, year, condition and city"/><br/><sub><b>Marketplace</b><br/>Used listings</sub></td>
    <td align="center"><img src="assets/screenshots/price-matic-listing-detail.webp" width="190" alt="Listing detail with condition, year, description and Contact Seller button"/><br/><sub><b>Listing</b><br/>Details and contact</sub></td>
    <td align="center"><img src="assets/screenshots/price-matic-seller-chat.webp" width="190" alt="Chat thread between buyer and seller"/><br/><sub><b>Seller chat</b><br/>Threaded messages</sub></td>
    <td align="center"><img src="assets/screenshots/price-matic-valuation-result.webp" width="190" alt="Valuation result with estimated value"/><br/><sub><b>Valuation</b><br/>ML price estimate</sub></td>
  </tr>
</table>

## Key Features

- **Browse by brand.** A brand grid and popular-appliance carousel on the home screen, with a category filter inside each brand.
- **Product detail.** Image gallery, rating, price, and collapsible key features and specification tables.
- **Side-by-side comparison.** Pick two appliances and view all specifications or only the differences, with differing values highlighted.
- **Reviews.** Read reviews per appliance or across the catalogue and write your own.
- **Used marketplace.** Listings with condition, year, city and photos, filters, and a Contact Seller action.
- **Seller chat.** One-to-one conversations tied to a listing, with threaded messages and image support.
- **Sell and manage.** A listing form with photo upload, plus My Ads with edit and delete.
- **Valuation.** Brand, condition, appliance type, working status, purchase year and warranty go to a FastAPI endpoint that returns an estimated price.
- **Admin screens.** Appliance inventory with add, edit and delete.

## Architecture

- The Expo app uses file-based routing (expo-router) and keeps auth state in React Context.
- A service layer wraps appliance, brand, chat, selling, image and user operations against Supabase (Postgres, Auth, Storage, Realtime).
- Price estimates are requested from a separate FastAPI service that loads a serialized scikit-learn pipeline.
- Spec data lives in type-specific tables (refrigerator, washing machine, AC) so comparison logic can rely on the right fields.

Details: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Engineering Highlights

- **Service-layer separation.** Screens call services rather than Supabase directly, which is what allowed an in-memory stand-in client to be swapped in for the demo build.
- **Per-type specification tables** instead of one generic JSON blob, with dynamic highlighting of differences in the comparison view.
- **End-to-end ML integration.** Client-side validation, a FastAPI endpoint and a scikit-learn pipeline (ordinal and one-hot encoding with linear regression) behind one screen.
- **Conversation de-duplication** per listing and user pair, so repeat enquiries reuse one thread.
- **Relational embedding** in Supabase queries to fetch reviews, users and listings together.
- **Realtime review channel** subscribed through Supabase.

More: [docs/ENGINEERING_CHALLENGES.md](docs/ENGINEERING_CHALLENGES.md) and [docs/TECHNICAL_CASE_STUDY.md](docs/TECHNICAL_CASE_STUDY.md).

## Status and Scope

- **Demo mode.** The screenshots come from a local build that uses an in-memory, fictional dataset instead of a live Supabase project. Listings, messages and reviews created in the demo are not persisted.
- **Real:** the UI, the service layer, the FastAPI endpoint and the scikit-learn model are the actual implementation, and the valuation screen calls the model end to end.
- **Demo-grade ML.** The model is trained on synthetically generated data with a hand-written price formula, so it reproduces that formula and is not market-trained.
- **Placeholder:** the "Check with AI" button on the sell screen is UI only; no other AI features are implemented.
- **Known rough edges:** the database schema file uses different column naming from the app code, and some backend dependencies are not pinned in the requirements file.
- Appliance images in the demo are generated illustrations.

## Documentation

- [Product overview](docs/PRODUCT_OVERVIEW.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Technical case study](docs/TECHNICAL_CASE_STUDY.md)
- [Engineering challenges](docs/ENGINEERING_CHALLENGES.md)
- [Privacy and source](docs/PRIVACY_AND_SOURCE.md)

## Rights

See [RIGHTS.md](RIGHTS.md).
