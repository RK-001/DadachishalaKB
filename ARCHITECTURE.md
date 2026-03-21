# Dada Chi Shala — Project Architecture

> **Organization:** Educare (Dada Chi Shala) Educational Trust  
> **Last Updated:** 2026-03-21

---

## 1. Project Overview

A full-featured NGO website for Educare (Dada Chi Shala) Educational Trust — a Pune-based organization providing free quality education to street and underprivileged children across Maharashtra.

**Two faces of the application:**
- **Public portal** — awareness, storytelling, events, gallery, volunteer registration, donations
- **Admin dashboard** — secure content management for all data entities

---

## 2. Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | React | 18.2 |
| Build Tool | Vite | 7.x |
| Styling | Tailwind CSS | 3.3 |
| Routing | react-router-dom | 6.15 (v7 future flags) |
| Server State | @tanstack/react-query | 5.x |
| Client State | React Context + Zustand | — |
| Forms | react-hook-form + yup | — |
| Animation | framer-motion | 12.x |
| Icons | lucide-react | — |
| Database | Firebase Firestore | — |
| Auth | Firebase Authentication | — |
| File Storage | Firebase Storage | — |
| Realtime Config | Firebase Realtime Database | — |
| Cloud Functions | Firebase Functions v2 | — |
| Analytics | Firebase Analytics | — |
| Payments | Razorpay | — |
| Email (client) | @emailjs/browser | — |
| Email (server) | nodemailer via Cloud Functions | — |
| SEO | react-helmet-async | — |

---

## 3. System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         BROWSER (SPA)                           │
│                                                                 │
│  ┌─────────────┐   ┌──────────────────┐   ┌─────────────────┐  │
│  │  Public     │   │  Admin Dashboard │   │  Maintenance    │  │
│  │  Pages      │   │  /admin/dashboard│   │  Page           │  │
│  └──────┬──────┘   └────────┬─────────┘   └────────┬────────┘  │
│         │                  │                       │           │
│         └──────────────────┼───────────────────────┘           │
│                            │                                   │
│              ┌─────────────▼──────────────┐                    │
│              │   React Query Data Layer   │                    │
│              │  useFirebaseQueries.js     │                    │
│              └─────────────┬──────────────┘                    │
│                            │                                   │
│              ┌─────────────▼──────────────┐                    │
│              │  cachedDatabaseService.js  │                    │
│              │  + cacheService.js         │                    │
│              │  (memory + localStorage)   │                    │
│              └─────────────┬──────────────┘                    │
└────────────────────────────┼────────────────────────────────────┘
                             │ Firebase SDK
┌────────────────────────────▼────────────────────────────────────┐
│                         FIREBASE                                │
│                                                                 │
│  ┌────────────┐  ┌──────────┐  ┌──────────┐  ┌─────────────┐  │
│  │ Firestore  │  │  Auth    │  │ Storage  │  │ Realtime DB │  │
│  │ (main DB)  │  │ (admin)  │  │ (images) │  │ (maint mode)│  │
│  └────────────┘  └──────────┘  └──────────┘  └─────────────┘  │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Cloud Functions (Node.js v2)               │   │
│  │  createRazorpayOrder · verifyRazorpayPayment            │   │
│  │  razorpayWebhook · sendDonationReceipt                  │   │
│  │  sendVolunteerConfirmation                              │   │
│  └──────────────────────┬──────────────────────────────────┘   │
└─────────────────────────┼───────────────────────────────────────┘
                          │
          ┌───────────────▼────────────┐
          │       RAZORPAY API         │
          │   (payment processing)     │
          └────────────────────────────┘
```

---

## 4. Folder Structure

```
(repo root)/
│
├── index.html                    # Vite HTML entry point
├── vite.config.js                # Vite config — chunks, aliases, ports
├── tailwind.config.js            # Tailwind theme — colors, spacing, screens
├── postcss.config.cjs            # PostCSS
├── firebase.json                 # Firebase Hosting config + rewrites
├── vercel.json                   # Vercel SPA fallback config
├── package.json
│
├── functions/                    # Firebase Cloud Functions (Node.js)
│   ├── index.js                  # All callable + HTTP functions
│   └── package.json
│
├── public/                       # Static assets (not processed by Vite)
│   ├── manifest.json             # PWA manifest
│   ├── robots.txt
│   ├── sitemap.xml
│   ├── icons/                    # Favicons + PWA icons
│   ├── images/                   # Static bitmap images
│   └── logos/                    # Brand logos
│
├── doc/                          # Knowledge Base module docs
│   ├── 01-public-storytelling-module.md
│   ├── 02-events-module.md
│   ├── 03-community-engagement-module.md
│   ├── 04-donations-fundraising-module.md
│   ├── 05-admin-content-management-module.md
│   ├── 06-platform-infrastructure-auth-module.md
│   └── quick-reference.md        # ← Fast lookup: collections, hooks, envs
│
└── src/                          # Application source
    ├── App.jsx                   # Root: providers, router, maintenance check
    ├── main.jsx                  # React DOM render entry
    ├── index.css                 # Tailwind directives + custom animations
    │
    ├── pages/                    # Route-level components (all lazy-loaded)
    │   ├── HomePage.jsx          # Hero, counters, events, stories, awards
    │   ├── AboutPage.jsx         # Mission, vision, focus areas
    │   ├── BranchesPage.jsx      # Branch listing + hero slider
    │   ├── TeamPage.jsx          # Team members with social links
    │   ├── GalleryPage.jsx       # Photos, videos, blogs, awards
    │   ├── EventsPage.jsx        # Event listing with status badges
    │   ├── DonatePage.jsx        # Razorpay + bank transfer
    │   ├── VolunteerPage.jsx     # 4-step registration form
    │   ├── ContactPage.jsx       # EmailJS form + map
    │   ├── MediaPage.jsx         # News, videos, press coverage
    │   ├── AdminLogin.jsx        # Firebase Auth login
    │   ├── AdminDashboard.jsx    # Full admin panel (tabbed)
    │   ├── MaintenancePage.jsx   # Shown when RTDB killswitch is ON
    │   └── NotFoundPage.jsx      # 404
    │
    ├── components/               # Reusable UI components
    │   ├── Navbar.jsx            # Top nav with mobile hamburger
    │   ├── Footer.jsx            # Site footer
    │   ├── ProtectedRoute.jsx    # Auth guard for admin routes
    │   ├── ErrorBoundary.jsx     # Top-level error catch + retry
    │   ├── ScrollToTop.jsx       # Auto-scroll on route change
    │   ├── SEO.jsx               # react-helmet-async wrapper
    │   ├── AnimatedCounter.jsx   # Impact number counter animation
    │   ├── ImageUpload.jsx       # Firebase Storage upload widget
    │   ├── AdminSetup.jsx        # One-time admin account bootstrap
    │   │
    │   ├── common/               # Generic shared primitives
    │   │   ├── Button.jsx
    │   │   ├── FormInput.jsx
    │   │   └── index.js          # Barrel export
    │   │
    │   ├── gallery/              # Gallery sub-components
    │   ├── stories/              # Stories/Testimonials sub-components
    │   └── team/                 # Team sub-components
    │       (+ domain-specific cards, forms, modals)
    │   ├── EventCard.jsx / EventForm.jsx / EventDetails.jsx
    │   ├── GalleryCard.jsx / GalleryForm.jsx / GalleryGrid.jsx
    │   ├── BlogCard.jsx / BlogModal.jsx
    │   ├── BranchCard.jsx
    │   └── Card.jsx              # Generic content card
    │
    │   (Admin management panels — one per entity)
    │   ├── EventManagement.jsx
    │   ├── GalleryManagement.jsx
    │   ├── BlogManagement.jsx
    │   ├── BranchManagement.jsx
    │   ├── TeamManagement.jsx
    │   ├── StoriesTestimonialsManagement.jsx
    │   ├── VolunteerManagement.jsx
    │   └── DonationManagement.jsx
    │
    ├── hooks/                    # Custom React hooks
    │   ├── useFirebaseQueries.js # ALL React Query hooks (reads + mutations)
    │   ├── useCRUD.js            # Generic CRUD helper hook
    │   └── useFirestore.js       # Low-level Firestore hook
    │
    ├── services/                 # External service integrations
    │   ├── firebase.js           # Firebase app init + service exports
    │   ├── cachedDatabaseService.js  # Firestore abstraction (all writes go here)
    │   ├── cacheService.js       # Dual-layer cache (memory + localStorage)
    │   ├── imageUploadService.js # Firebase Storage upload helpers
    │   ├── emailService.js       # EmailJS client-side email
    │   └── razorpayService.js    # Razorpay SDK helpers
    │
    ├── context/                  # React Context providers
    │   ├── AuthContext.jsx       # Firebase Auth state + login/logout methods
    │   └── NotificationContext.jsx # Global toast/notification state
    │
    ├── config/                   # App-level configuration
    │   ├── colors.js             # Shared color constants
    │   └── queryClient.jsx       # React Query client setup (5-min stale time)
    │
    └── utils/                    # Pure utility functions
        ├── sanitization.js       # XSS prevention — sanitizeString, sanitizeEmail, etc.
        ├── validators.js         # Yup-based schema validators
        ├── formatters.js         # Date, currency, text formatters
        ├── helpers.js            # General-purpose helpers
        ├── colorUtils.js         # Color manipulation utilities
        ├── logger.js             # Logging abstraction
        └── adminSetup.js         # One-time admin user bootstrap utility
```

---

## 5. Routing Architecture

All routes are defined in `src/App.jsx` using react-router-dom v6.

```
/                    → HomePage          (public)
/about               → AboutPage         (public)
/branches            → BranchesPage      (public)
/team                → TeamPage          (public)
/gallery             → GalleryPage       (public)
/events              → EventsPage        (public)
/donate              → DonatePage        (public)
/volunteer           → VolunteerPage     (public)
/contact             → ContactPage       (public)
/media               → MediaPage         (public)
/admin               → redirect → /admin/login
/admin/login         → AdminLogin        (public)
/admin/dashboard     → AdminDashboard    (ProtectedRoute — Firebase Auth required)
*                    → NotFoundPage
```

**Maintenance mode override:** When `config/maintenanceMode = true` in Firebase RTDB (production only), the entire app renders `MaintenancePage`. Admin routes still accessible in development.

---

## 6. Data Flow Architecture

### Read Path

```
Component
  └─► useXxx() hook  (useFirebaseQueries.js)
        └─► React Query  (cache check: staleTime 1–5 min)
              └─► cachedDatabaseService.getXxx()
                    └─► cacheService  (memory → localStorage check)
                          └─► Firestore SDK  (on cache miss)
```

### Write Path

```
Component / Form
  └─► useAddXxx() / useUpdateXxx() mutation  (useFirebaseQueries.js)
        └─► cachedDatabaseService.addXxx() / updateXxx()
              ├─► Firestore SDK  (addDoc / updateDoc / deleteDoc)
              ├─► cacheService.invalidateCollection()
              └─► queryClient.invalidateQueries()  (React Query cache bust)
```

### Donation Payment Flow

```
User fills form
  └─► razorpayService  (calls Cloud Function: createRazorpayOrder)
        └─► Cloud Function creates Razorpay order + pending Firestore doc
              └─► Razorpay checkout opens (client-side SDK)
                    └─► On success: Cloud Function verifyRazorpayPayment
                          ├─► HMAC signature verification (server-side)
                          ├─► Firestore donation doc → status: "completed"
                          ├─► Creates donor record in `donors` collection
                          └─► sendDonationReceipt (email via nodemailer)
```

---

## 7. Firestore Collections Summary

| Collection | Purpose | Key Fields |
|-----------|---------|-----------|
| `events` | Events listing | `title`, `event_date`, `status` |
| `gallery` | Photos / videos / awards | `url`, `category`, `type` |
| `successStories` | Student success stories | `name`, `story`, `image_url` |
| `testimonials` | Supporter testimonials | `name`, `role`, `message` |
| `blogs` | Blog posts | `title`, `content`, `published` |
| `team` | Team member profiles | `name`, `role`, `category`, `order` |
| `awards` | Award recognitions | `title`, `year`, `organization` |
| `news` | News / press coverage | `title`, `url`, `source` |
| `videos` | Video embeds | `title`, `embed_url`, `category` |
| `branches` | Branch locations | `name`, `location`, `coordinator` |
| `volunteers` | Volunteer applications | `personal_info`, `application_status` |
| `donations` | Payment records | `razorpayOrderId`, `amount`, `status` |
| `donors` | Donor registry | `name`, `email`, `panNumber` |

**Realtime Database (RTDB):**

| Path | Type | Purpose |
|------|------|---------|
| `config/maintenanceMode` | boolean | Site-wide killswitch |

---

## 8. Cloud Functions (functions/index.js)

All functions use Firebase Functions v2 (`firebase-functions/v2`). Secrets are env vars — never exposed to client.

| Function | Type | Purpose |
|---------|------|---------|
| `createRazorpayOrder` | `onCall` | Create Razorpay order + pending donation doc |
| `verifyRazorpayPayment` | `onCall` | HMAC verify + mark donation completed |
| `razorpayWebhook` | `onRequest` | Webhook handler for async Razorpay events |
| `sendDonationReceipt` | `onCall` | Send receipt email via nodemailer |
| `sendVolunteerConfirmation` | `onCall` | Send confirmation email to new volunteer |

---

## 9. Caching Strategy

**Dual-layer architecture (`cacheService.js`):**

| Layer | Storage | Scope | TTL |
|-------|---------|-------|-----|
| L1 | In-memory Map | Process lifetime | Configurable |
| L2 | localStorage | Browser session | Configurable |

**React Query on top** provides an additional query-level cache with collection-specific stale times:
- Events / upcoming events: **1 minute**
- Gallery: **2 minutes**
- Other collections: **5 minutes** (React Query default)

On any mutation, `cacheService.invalidateCollection()` + `queryClient.invalidateQueries()` both fire to keep all layers in sync.

---

## 10. Build & Deployment

### Build Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start Vite dev server on port 3000 |
| `npm run build` | Production build → `dist/` |
| `npm run build-vercel` | Vercel deployment build |

### Code Splitting Chunks (Vite rollupOptions)

| Chunk | Modules |
|-------|---------|
| `vendor` | `react`, `react-dom` |
| `firebase` | Firebase app, Firestore, Auth, Storage |
| `ui` | framer-motion, lucide-react, react-modal |
| `forms` | react-hook-form, @hookform/resolvers, yup |
| `utils` | @emailjs/browser |

### Deployment Targets

| Target | Config File | Notes |
|--------|------------|-------|
| Firebase Hosting | `firebase.json` | SPA rewrites → `index.html` |
| Vercel | `vercel.json` | SPA fallback rewrite |

### Environment Variables (all `VITE_` prefixed for client)

| Variable | Required |
|---------|---------|
| `VITE_FIREBASE_API_KEY` | Yes |
| `VITE_FIREBASE_AUTH_DOMAIN` | Yes |
| `VITE_FIREBASE_PROJECT_ID` | Yes |
| `VITE_FIREBASE_STORAGE_BUCKET` | Yes |
| `VITE_FIREBASE_MESSAGING_SENDER_ID` | Yes |
| `VITE_FIREBASE_APP_ID` | Yes |
| `VITE_FIREBASE_MEASUREMENT_ID` | Optional |
| `VITE_FIREBASE_DATABASE_URL` | Optional (auto-derived) |
| `VITE_RAZORPAY_KEY_ID` | Yes (client-side key only) |
| `VITE_EMAILJS_*` | Yes (EmailJS config) |

Cloud Function secrets (`RAZORPAY_KEY_SECRET`, `EMAIL_PASSWORD`, etc.) are set via Firebase env config — never in client code.

---

## 11. Coding Standards

### Component Conventions

| Rule | Standard |
|------|---------|
| Naming | PascalCase for components (`EventCard.jsx`) |
| Hooks | `useXxx` prefix (`useEvents`, `useAddEvent`) |
| Services | camelCase functions (`getEvents`, `addEvent`) |
| Pages | Lazy-loaded with `React.lazy()` + `Suspense` |
| Admin routes | Always wrapped in `<ProtectedRoute>` |

### Data Access Rules

| Operation | Required Approach |
|-----------|-----------------|
| Firestore reads | React Query hooks from `useFirebaseQueries.js` |
| Firestore writes | Through `cachedDatabaseService.js` methods |
| Direct Firestore SDK in components | **Forbidden** |
| After any mutation | `queryClient.invalidateQueries()` required |

### Forms

- Library: `react-hook-form` + `@hookform/resolvers` + `yup`
- All user input sanitized before Firestore write via `src/utils/sanitization.js`
- Sanitizers available: `sanitizeString`, `sanitizeEmail`, `sanitizePhone`, `sanitizeUrl`, `sanitizeObject`

### Security Rules

| Concern | Enforcement |
|---------|-----------|
| XSS prevention | All user input through `sanitization.js` before DB write |
| Admin auth | `ProtectedRoute` on all `/admin/dashboard` routes |
| Secrets | Razorpay key secret, email passwords — Cloud Functions env only |
| HMAC verification | Razorpay payment signature verified server-side only |
| Protocol validation | `sanitizeUrl()` allows only `http:` / `https:` |

### Styling

- **Tailwind CSS** utility classes only — no separate CSS files except `index.css` for custom animations
- Theme colors defined in `tailwind.config.js`:
  - `primary` — Dark Purple-Blue (`#191947`)
  - `secondary` — Warm Orange/Gold (`#eba645`)
  - `neutral` — Custom gray scale
  - Semantic: `success`, `warning`, `error`
- Mobile-first responsive design; custom `xs` breakpoint at `475px`

### Error Handling

- Top-level `<ErrorBoundary>` in `App.jsx` catches render errors with retry UI
- Email/notification failures must not block data writes
- Firebase init failures default gracefully (site loads normally, maintenance defaults `false`)

### Comments & Documentation

- Comments only where logic is non-obvious
- No docstrings on every function
- KB files in `doc/` are the source of truth — update them when requirements change

---

## 12. Authentication Flow

```
User hits /admin/dashboard
  └─► ProtectedRoute checks AuthContext.currentUser
        ├─► null  → redirect to /admin/login
        └─► authenticated → render AdminDashboard

/admin/login
  └─► Firebase Auth: signInWithEmailAndPassword
        └─► AuthContext stores user
              └─► redirect to /admin/dashboard
```

Only one admin account is intended. Account bootstrapped via `src/utils/adminSetup.js` + `AdminSetup.jsx` (one-time use).

---

## 13. Key Architectural Decisions

| Decision | Rationale |
|---------|-----------|
| Single SPA for public + admin | Simpler deployment; code-split mitigates bundle size |
| React Query as data layer | Automatic caching, background refetch, optimistic updates |
| `cachedDatabaseService` abstraction | Single place to add/modify Firestore logic; components stay pure |
| Dual-layer cache (memory + localStorage) | Reduces Firestore read costs; fast repeat page loads |
| Cloud Functions for payment | Razorpay secret never touches client; HMAC verify is server-only |
| Firebase Realtime DB for maintenance | Instant toggle without re-deploy; zero polling cost |
| `VITE_` env var prefix | Vite requirement; prevents accidental server-secret leakage |
