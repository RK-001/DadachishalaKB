# Dada Chi Shala Knowledge Base and Project Guide

Organization: Educare Dada Chi Shala Educational Trust
Scope: NGO website with public portal, admin dashboard, and cloud backend
Tech foundation: React 18, Vite, Tailwind CSS, Firebase, Razorpay
Last updated: 2026-03-21

## Agent starting point

This README is the entry point for anyone using this knowledge base.

Read in this order:

1. INDEX.md for navigation, keyword lookup, and routing.
2. The target module file identified by INDEX.md.
3. qc_quick_reference.md for collections, hooks, services, environment variables, and common issues.
4. ARCHITECTURE.md when the task needs system level context.

Do not guess field names, collection names, hooks, or service methods. Look them up in qc_quick_reference.md first.

## Project summary

This knowledge base describes a React and Firebase application for Educare Dada Chi Shala Educational Trust. The website supports public storytelling, events, branches, volunteers, donations, media, and an admin dashboard for content management.

## Main capability areas

Public experience

- Home page with storytelling, counters, events, stories, and awards.
- About page with mission, vision, and impact.
- Branches page with branch information.
- Team page with founders, team members, and volunteers.
- Gallery and media pages for photos, videos, blogs, awards, and press coverage.
- Events page for event listings.
- Donate page for Razorpay and manual transfer flows.
- Volunteer page for a four step registration form.
- Contact page for organization contact workflows.

Admin experience

- Firebase Auth based login.
- Admin dashboard with management panels for events, gallery, blogs, branches, team, stories, testimonials, volunteers, and donations.

Technical features

- Route level code splitting with React.lazy and Suspense.
- Error boundary and scroll restoration.
- Maintenance mode through Firebase Realtime Database.
- SEO metadata support.
- React Query for data fetching and cache refresh.
- Dual layer caching through memory and localStorage.
- Input sanitization before database writes.

## Knowledge base file map

- 01-public-storytelling-module.md for public pages and storytelling content.
- 02-events-module.md for event listing and event management.
- 03-community-engagement-module.md for volunteers and branches.
- 04-donations-fundraising-module.md for donation and payment flows.
- 05-admin-content-management-module.md for the admin dashboard and CRUD screens.
- 06-platform-infrastructure-auth-module.md for routing, auth, Firebase setup, and shared services.
- ARCHITECTURE.md for system level structure and data flow.
- qc_quick_reference.md for fast field, hook, service, and environment lookup.
- copilot-instructions.md for repository working rules.
- INDEX.md for navigation.

## Setup summary

Prerequisites

- Node.js 20.19 or newer, or Node.js 22.12 or newer.
- Firebase project.
- EmailJS account if the contact flow uses it.
- Razorpay account if online payments are enabled.

Install

1. Clone the repository.
2. Open the project root.
3. Run npm install.

Required client environment values

- VITE_FIREBASE_API_KEY
- VITE_FIREBASE_AUTH_DOMAIN
- VITE_FIREBASE_PROJECT_ID
- VITE_FIREBASE_STORAGE_BUCKET
- VITE_FIREBASE_MESSAGING_SENDER_ID
- VITE_FIREBASE_APP_ID
- VITE_FIREBASE_DATABASE_URL when Realtime Database URL is not auto derived
- VITE_EMAILJS_SERVICE_ID when EmailJS is used
- VITE_EMAILJS_TEMPLATE_ID when EmailJS is used
- VITE_EMAILJS_PUBLIC_KEY when EmailJS is used
- VITE_RAZORPAY_KEY_ID when Razorpay checkout is used

Firebase setup summary

1. Create a Firebase project.
2. Enable Firestore Database.
3. Enable Authentication with Email and Password.
4. Enable Storage.
5. Enable Realtime Database for maintenance mode.
6. Deploy Cloud Functions if Razorpay or server side email is required.

Run commands

- npm run dev for development.
- npm run build for production build.
- firebase deploy or vercel deploy depending on the hosting target.

## Primary collections

- events
- branches
- gallery
- successStories
- testimonials
- blogs
- awards
- news
- videos
- volunteers
- donations
- donors
- team

## Deployment summary

- Firebase Hosting and Vercel both rely on SPA rewrites.
- Keep client side secrets out of the repository.
- Keep payment and email secrets in server side function configuration only.
