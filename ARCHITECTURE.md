# Project Architecture

Organization: Educare Dada Chi Shala Educational Trust
Last updated: 2026-03-21

## 1. Project overview

This is a full featured NGO website for Educare Dada Chi Shala Educational Trust, a Pune based organization providing free quality education to street and underprivileged children across Maharashtra.

The application has two main faces.

- Public portal for awareness, storytelling, events, gallery, volunteer registration, and donations.
- Admin dashboard for secure content management.

## 2. Tech stack

- Framework: React 18.2
- Build tool: Vite 7.x
- Styling: Tailwind CSS 3.3
- Routing: react-router-dom 6.15
- Server state: @tanstack/react-query 5.x
- Client state: React Context and Zustand
- Forms: react-hook-form and yup
- Animation: framer-motion 12.x
- Icons: lucide-react
- Database: Firebase Firestore
- Auth: Firebase Authentication
- File storage: Firebase Storage
- Realtime config: Firebase Realtime Database
- Cloud Functions: Firebase Functions v2
- Analytics: Firebase Analytics
- Payments: Razorpay
- Client email: @emailjs/browser
- Server email: nodemailer via Cloud Functions
- SEO: react-helmet-async

## 3. System architecture

```text
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

## 4. Folder structure

Top level files and folders

- index.html for the Vite HTML entry point
- vite.config.js for build configuration
- tailwind.config.js for theme configuration
- postcss.config.cjs for PostCSS
- firebase.json for Firebase Hosting rewrites
- vercel.json for Vercel SPA fallback
- package.json
- functions for Firebase Cloud Functions
- public for static assets
- src for application source
- markdown knowledge base files at the repository root

## 5. Routing architecture

All routes are defined in src/App.jsx using react-router-dom v6.

```text
/                    -> HomePage
/about               -> AboutPage
/branches            -> BranchesPage
/team                -> TeamPage
/gallery             -> GalleryPage
/events              -> EventsPage
/donate              -> DonatePage
/volunteer           -> VolunteerPage
/contact             -> ContactPage
/media               -> MediaPage
/admin               -> redirect to /admin/login
/admin/login         -> AdminLogin
/admin/dashboard     -> AdminDashboard through ProtectedRoute
*                    -> NotFoundPage
```

When config/maintenanceMode is true in Firebase Realtime Database in production, the entire app renders MaintenancePage.

## 6. Data flow architecture

Read path

```text
Component
  └─► useXxx() hook  (useFirebaseQueries.js)
        └─► React Query  (cache check: staleTime 1–5 min)
              └─► cachedDatabaseService.getXxx()
                    └─► cacheService  (memory → localStorage check)
                          └─► Firestore SDK  (on cache miss)
```

Write path

```text
Component / Form
  └─► useAddXxx() / useUpdateXxx() mutation  (useFirebaseQueries.js)
        └─► cachedDatabaseService.addXxx() / updateXxx()
              ├─► Firestore SDK  (addDoc / updateDoc / deleteDoc)
              ├─► cacheService.invalidateCollection()
              └─► queryClient.invalidateQueries()  (React Query cache bust)
```

Donation payment flow

```text
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

## 7. Firestore collections summary

- events for event listings with fields such as title, event_date, and status
- gallery for photos, videos, and awards with fields such as url, category, and type
- successStories for student stories with fields such as name, story, and image_url
- testimonials for supporter testimonials with fields such as name, role, and message
- blogs for blog posts with fields such as title, content, and published
- team for team profiles with fields such as name, role, category, and order
- awards for award recognitions with fields such as title, year, and organization
- news for press coverage with fields such as title, url, and source
- videos for video embeds with fields such as title, embed_url, and category
- branches for branch locations with fields such as name, location, and coordinator
- volunteers for volunteer applications with fields such as personal_info and application_status
- donations for payment records with fields such as razorpayOrderId, amount, and status
- donors for donor records with fields such as name, email, and panNumber
- Realtime Database path config/maintenanceMode as a site wide kill switch

## 8. Cloud Functions

The main functions in functions/index.js are:

- createRazorpayOrder using onCall to create an order and pending donation record
- verifyRazorpayPayment using onCall to verify HMAC and mark a donation complete
- razorpayWebhook using onRequest for asynchronous Razorpay events
- sendDonationReceipt using onCall to send receipt email
- sendVolunteerConfirmation using onCall to send volunteer confirmation email

Secrets remain in environment configuration and must never be exposed to the client.

## 9. Caching strategy

The cache design has two layers in cacheService.js.

- L1 is an in memory map for process lifetime.
- L2 is localStorage for browser session persistence.

React Query adds another cache layer with collection specific stale times.

- Events and upcoming events use about 1 minute.
- Gallery uses about 2 minutes.
- Other collections use the 5 minute default.

On any mutation, cacheService.invalidateCollection() and queryClient.invalidateQueries() run together.

## 10. Build and deployment

Build commands

- npm run dev starts the Vite dev server.
- npm run build creates the production build in dist.
- npm run build-vercel creates the Vercel build.

Deployment targets

- Firebase Hosting using firebase.json for SPA rewrites to index.html.
- Vercel using vercel.json for SPA fallback rewrite.

Client environment variables

- VITE_FIREBASE_API_KEY required
- VITE_FIREBASE_AUTH_DOMAIN required
- VITE_FIREBASE_PROJECT_ID required
- VITE_FIREBASE_STORAGE_BUCKET required
- VITE_FIREBASE_MESSAGING_SENDER_ID required
- VITE_FIREBASE_APP_ID required
- VITE_FIREBASE_MEASUREMENT_ID optional
- VITE_FIREBASE_DATABASE_URL optional
- VITE_RAZORPAY_KEY_ID required for client side checkout only
- VITE_EMAILJS variables for EmailJS configuration

Cloud Function secrets such as RAZORPAY_KEY_SECRET and EMAIL_PASSWORD remain server side.

## 11. Coding standards

Component conventions

- Use PascalCase for components.
- Use the useXxx prefix for hooks.
- Use camelCase for service functions.
- Lazy load pages with React.lazy() and Suspense.
- Always wrap admin routes in ProtectedRoute.

Data access rules

- Firestore reads must use hooks from useFirebaseQueries.js.
- Firestore writes must go through cachedDatabaseService.js methods.
- Direct Firestore SDK calls in components are forbidden.
- Every mutation must invalidate related query keys.

Forms and security

- Use react-hook-form, resolvers, and yup.
- Sanitize user input before Firestore writes through src/utils/sanitization.js.
- Keep payment secrets in Cloud Functions only.
- Verify Razorpay payment signatures server side only.
- Restrict URLs to safe protocols.

Styling and errors

- Use Tailwind CSS utility classes and keep custom styling in index.css where needed.
- Use the configured color system in tailwind.config.js.
- ErrorBoundary in App.jsx handles render failures.
- Email and notification failures must not block data writes.

## 12. Authentication flow

```text
User hits /admin/dashboard
  └─► ProtectedRoute checks AuthContext.currentUser
        ├─► null  → redirect to /admin/login
        └─► authenticated → render AdminDashboard

/admin/login
  └─► Firebase Auth: signInWithEmailAndPassword
        └─► AuthContext stores user
              └─► redirect to /admin/dashboard
```

Only one admin account is intended. It is bootstrapped through src/utils/adminSetup.js and AdminSetup.jsx for one time use.

## 13. Key architectural decisions

- Single SPA for public and admin to simplify deployment, with code splitting to control bundle size.
- React Query as the data layer for caching and background refetch.
- cachedDatabaseService as the Firestore abstraction so components stay simpler.
- Dual layer cache using memory and localStorage to reduce Firestore reads.
- Cloud Functions for payment so secrets never touch the client and HMAC verification stays server side.
- Firebase Realtime Database for maintenance mode so the site can be toggled without redeploy.
- VITE prefixed variables for client configuration so server secrets are not mixed into client code.
