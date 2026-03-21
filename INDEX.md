# KB Index Workflow Navigation

Purpose: Navigation index for the knowledge base and for any agent using keyword lookup.
Organization: Educare Dada Chi Shala Educational Trust
Repository: Dada Chi Shala NGO Website React and Firebase SPA
Last updated: 2026-03-21

How to use this index

1. Find the module or keyword that matches the topic.
2. Open the listed file.
3. Validate collection names, field names, hooks, and service methods in qc_quick_reference.md.
4. If the topic is not covered, call out the gap instead of guessing.

Module entries

Public information and storytelling

- File: 01-public-storytelling-module.md
- Keywords: homepage, about, gallery, team, media, contact, blog, stories, testimonials, awards, news, videos, seo, hero, AnimatedCounter, successStories, GalleryPage, TeamPage, MediaPage, AboutPage, ContactPage
- Covers: Public facing pages, storytelling content, content display components, related collections, and SEO behavior.

Events management

- File: 02-events-module.md
- Keywords: events, EventsPage, EventCard, EventForm, EventDetails, EventManagement, upcoming events, useUpcomingEvents, events collection, event_date, admin event CRUD
- Covers: Public events listing, upcoming event logic, and admin CRUD for events.

Community engagement volunteers and branches

- File: 03-community-engagement-module.md
- Keywords: volunteer, VolunteerPage, VolunteerManagement, volunteers collection, branch, branches, BranchesPage, BranchCard, BranchManagement, four step form
- Covers: Volunteer registration flow, admin volunteer review, branch listing, and branch CRUD.

Donations and fundraising

- File: 04-donations-fundraising-module.md
- Keywords: donate, donations, DonatePage, DonationManagement, Razorpay, razorpayService, createRazorpayOrder, verifyRazorpayPayment, razorpayWebhook, donors, payment, bank transfer, online donation
- Covers: Donation flow, Razorpay order and verification flow, webhook handling, donor records, receipts, and admin donation management.

Admin content management

- File: 05-admin-content-management-module.md
- Keywords: admin dashboard, AdminDashboard, admin CRUD, GalleryManagement, TeamManagement, BlogManagement, StoriesTestimonialsManagement, DonationManagement, VolunteerManagement, BranchManagement, EventManagement, ImageUpload, admin panel
- Covers: Admin dashboard layout, tabs, image upload, and CRUD screens for managed content.

Platform infrastructure and auth

- File: 06-platform-infrastructure-auth-module.md
- Keywords: App.jsx, router, routes, Firebase, Firestore, Auth, AuthContext, Firebase Storage, Realtime Database, maintenance mode, MaintenancePage, AdminLogin, ProtectedRoute, ErrorBoundary, ScrollToTop, NotificationContext, code splitting, lazy loading, firebase.json, vercel.json, vite.config.js, tailwind.config.js, deployment, NotFoundPage
- Covers: App shell, routing, Firebase initialization, authentication, protected routes, maintenance mode, deployment configuration, and build tooling.

Quick reference

- File: qc_quick_reference.md
- Keywords: Firestore collections, data model, fields, schema, useFirebaseQueries, useCRUD, useFirestore, queryClient, QUERY_KEYS, React Query, cachedDatabaseService, cacheService, emailService, imageUploadService, firebase.js, sanitization, validators, helpers, logger, environment variables, VITE_FIREBASE, build commands, error patterns, cache strategy
- Covers: Collection fields, hooks, service methods, utility notes, environment variables, common issues, and build commands.

Project architecture

- File: ARCHITECTURE.md
- Keywords: architecture, project overview, tech stack, folder structure, system diagram, data flow, read path, write path, routing architecture, caching strategy, deployment targets, coding standards, security rules, authentication flow, architectural decisions
- Covers: High level system structure, diagrams, routing, data flow, collection summary, cloud functions, caching strategy, build and deployment, and coding standards.

Collection to module map

- events -> 02-events-module.md
- volunteers -> 03-community-engagement-module.md
- branches -> 03-community-engagement-module.md
- donations -> 04-donations-fundraising-module.md
- donors -> 04-donations-fundraising-module.md
- gallery -> 01-public-storytelling-module.md
- successStories -> 01-public-storytelling-module.md
- testimonials -> 01-public-storytelling-module.md
- blogs -> 01-public-storytelling-module.md
- team -> 01-public-storytelling-module.md
- awards -> 01-public-storytelling-module.md
- news -> 01-public-storytelling-module.md
- videos -> 01-public-storytelling-module.md
- config/maintenanceMode -> 06-platform-infrastructure-auth-module.md

Cross reference by task

- Donation feature work: 04-donations-fundraising-module.md, then qc_quick_reference.md.
- Event feature work: 02-events-module.md, then qc_quick_reference.md.
- Volunteer flow work: 03-community-engagement-module.md, then qc_quick_reference.md.
- Admin dashboard changes: 05-admin-content-management-module.md, then 06-platform-infrastructure-auth-module.md.
- Gallery or media changes: 01-public-storytelling-module.md, then 05-admin-content-management-module.md.
- Authentication or protected route changes: 06-platform-infrastructure-auth-module.md.
- Performance or caching work: qc_quick_reference.md, then ARCHITECTURE.md.
- SEO changes: 01-public-storytelling-module.md, then qc_quick_reference.md.
- Deployment or environment setup: qc_quick_reference.md, then 06-platform-infrastructure-auth-module.md.
- Architecture overview: ARCHITECTURE.md, then qc_quick_reference.md.

Route map

- / -> HomePage.jsx -> 01-public-storytelling-module.md
- /about -> AboutPage.jsx -> 01-public-storytelling-module.md
- /gallery -> GalleryPage.jsx -> 01-public-storytelling-module.md
- /team -> TeamPage.jsx -> 01-public-storytelling-module.md
- /media -> MediaPage.jsx -> 01-public-storytelling-module.md
- /events -> EventsPage.jsx -> 02-events-module.md
- /branches -> BranchesPage.jsx -> 03-community-engagement-module.md
- /volunteer -> VolunteerPage.jsx -> 03-community-engagement-module.md
- /donate -> DonatePage.jsx -> 04-donations-fundraising-module.md
- /contact -> ContactPage.jsx -> 01-public-storytelling-module.md
- /admin and /admin/login -> AdminLogin.jsx -> 06-platform-infrastructure-auth-module.md
- /admin/dashboard -> AdminDashboard.jsx -> 05-admin-content-management-module.md
- all other routes -> NotFoundPage.jsx -> 06-platform-infrastructure-auth-module.md
