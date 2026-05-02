**# Rate-My-Wall**# ⬡ RateMyWall — Climbing Gym Directory

A full-featured climbing gym directory built with Vite + React, Firebase, and Leaflet. Dark theme matching The Climbing Wall App's design.

[**Try the live demo →**](https://ratemywall.com)

---

## Stack

| Layer | Tech |
|-------|------|
| Frontend | Vite + React 18, React Router v6, CSS Modules |
| Auth | Firebase Authentication (Email + Google OAuth) |
| Database | Cloud Firestore |
| Maps | Leaflet + React-Leaflet (OpenStreetMap/CartoDB dark tiles) |
| SEO | React Helmet Async + JSON-LD structured data |
| Hosting | Vercel |
| Notifications | React Hot Toast |

---

## Project Structure

```
src/
├── components/
│   ├── auth/          # ProtectedRoute
│   ├── gym/           # GymCard
│   ├── layout/        # Navbar, Footer
│   ├── map/           # GymMap (Leaflet)
│   ├── reviews/       # ReviewForm, ReviewList
│   └── ui/            # SEO, StarRating
├── context/
│   └── AuthContext.jsx
├── lib/
│   ├── firebase.js    # Firebase init
│   └── firestore.js   # Data access layer + seed data
├── pages/
│   ├── HomePage.jsx
│   ├── GymsPage.jsx
│   ├── GymProfilePage.jsx
│   ├── SubmitGymPage.jsx
│   ├── LoginPage.jsx
│   ├── SignUpPage.jsx
│   ├── AboutPage.jsx
│   └── NotFoundPage.jsx
└── styles/
    └── globals.css
```

---

## Firestore Data Model

### `gyms` collection
| Field | Type | Notes |
|-------|------|-------|
| `name` | string | Display name |
| `slug` | string | URL identifier, must be unique |
| `city`, `state` | string | Location |
| `lat`, `lng` | number | For map pins |
| `climbingTypes` | string[] | `bouldering`, `top-rope`, `lead` |
| `approved` | boolean | false = pending review |
| `featured` | boolean | Shows on homepage |
| `rating` | number | Aggregate, auto-updated on review |
| `reviewCount` | number | Auto-incremented |
| `nameSearch` | string[] | Lowercase word tokens for search |

### `reviews` collection
| Field | Type | Notes |
|-------|------|-------|
| `gymId` | string | Firestore doc ID of gym |
| `userId` | string | Firebase Auth UID |
| `authorName` | string | Display name at time of review |
| `rating` | number | 1–5 |
| `subRatings` | map | `settingQuality`, `gradeAccuracy`, etc. |
| `body` | string | Min 20 chars |
| `tags` | string[] | Community-selected tags |
| `createdAt` | timestamp | Server timestamp |

### `users` collection
| Field | Type | Notes |
|-------|------|-------|
| `uid` | string | Firebase Auth UID |
| `email` | string | |
| `displayName` | string | |
| `reviewCount` | number | |
| `createdAt` | timestamp | |

---

## SEO Architecture

Every page uses the `<SEO>` component (React Helmet Async) which sets:
- `<title>` — page-specific with site suffix
- `<meta name="description">` — unique per page
- `<link rel="canonical">` — absolute URL
- Open Graph tags for social sharing
- Twitter Card tags
- JSON-LD structured data

**Gym profile pages** include `SportsActivityLocation` schema with address, geo, phone, and `AggregateRating` — this enables rich snippets in Google search results (star ratings displayed in SERPs).

**Homepage** includes `WebSite` schema with `SearchAction` — tells Google about your search functionality.

---

## The Climbing Wall App Integration

RateMyWall is designed as a companion to [The Climbing Wall App](https://the-climbing-wall.phillippereira.com). Integration points:

- Footer persistent CTA linking to app landing page + Google Play
- CWA promo banner on homepage
- "Log this gym" CTA on every gym profile page
- About page dedicated section

---

## License

MIT
