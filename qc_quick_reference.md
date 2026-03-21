# Quick Reference for Dada Chi Shala Website

Purpose: Fast lookup for Firestore collections, React Query hooks, service methods, environment variables, and common issues.
Organization: Educare Dada Chi Shala Educational Trust
Last updated: 2026-03-21

## 1. Data model

All collections live in Firebase Firestore unless stated otherwise. Timestamps are Firestore Timestamp values unless noted.

events

- title as the event name
- description for the full event description
- event_date as an ISO date string used for ordering and upcoming filtering
- event_time for display time
- location for venue name or address
- location_url for an optional maps link
- category for event tagging
- image_url for an optional storage URL
- status such as upcoming, completed, or cancelled
- created_at and updated_at

Ordering uses event_date ascending. Upcoming filtering uses event_date greater than or equal to today.

gallery

- title
- description
- url
- category such as photos, videos, or awards
- type as image or video
- thumbnail_url when needed for video items
- uploaded_at and updated_at

successStories

- name
- story
- image_url
- year
- category
- created_at and updated_at

testimonials

- name
- role
- message
- image_url
- rating
- created_at and updated_at

blogs

- title
- content
- excerpt
- author
- author_type
- image_url
- tags
- published
- created_at and updated_at

team

- name
- role
- bio
- image_url
- category
- social_links
- order
- created_at and updated_at

awards

- title
- description
- year
- image_url
- organization
- created_at

news

- title
- excerpt
- url
- source
- date
- image_url
- created_at

videos

- title
- description
- embed_url
- thumbnail_url
- category
- created_at

volunteers

- personal_info with name, email, phone, address, date_of_birth, and gender
- skills_and_interests with skills, subjects, and languages
- availability with days, time_slots, and hours_per_week
- experience_and_motivation with motivation, prior_experience, and education
- references_and_emergency with reference_name, reference_contact, and emergency_contact
- application_status
- status as a mirror field
- assigned_branch
- branch_coordinator_email
- action_history
- created_at and updated_at

branches

- name
- location
- address
- timings
- coordinator
- coordinator_email
- image_url
- hero_image_url
- student_count
- established_year
- created_at and updated_at

donations

- razorpayOrderId
- razorpayPaymentId
- amount
- status
- donorInfo with name, email, phone, and panNumber
- donationType
- message
- paymentMethod
- receipt_url
- createdAt and updatedAt

donors

- name
- email
- phone
- panNumber
- amount
- donationType
- paymentId
- orderId
- donationId
- receiptSent
- createdAt

Realtime Database

- config/maintenanceMode as a boolean flag that shows MaintenancePage in production when true

## 2. Hooks and services

Query keys in useFirebaseQueries.js include:

- events
- upcomingEvents
- gallery
- successStories
- testimonials
- blogs
- blog
- branches
- branch
- awards
- news
- videos
- team
- volunteers
- donations
- donation
- donationStats

Common React Query hooks

Events

- useEvents(limitCount)
- useUpcomingEvents(limitCount = 3)
- useAddEvent()
- useUpdateEvent()
- useDeleteEvent()

Gallery

- useGalleryItems(category, limitCount)
- useAddGalleryItem()
- useUpdateGalleryItem()
- useDeleteGalleryItem()

Stories and testimonials

- useSuccessStories()
- useAddSuccessStory()
- useUpdateSuccessStory()
- useDeleteSuccessStory()
- useTestimonials()
- useAddTestimonial()
- useUpdateTestimonial()
- useDeleteTestimonial()

Blogs

- useBlogs(authorType, limitCount)
- useBlog(blogId)
- useAddBlog()
- useUpdateBlog()
- useDeleteBlog()

Branches

- useBranches()
- useBranch(branchId)
- useAddBranch()
- useUpdateBranch()
- useDeleteBranch()

Other managed domains follow the same useX, useAddX, useUpdateX, and useDeleteX pattern.

cachedDatabaseService.js method inventory

- getEvents, getUpcomingEvents, addEvent, updateEvent, deleteEvent
- getGalleryItems, addGalleryItem, updateGalleryItem, deleteGalleryItem
- getSuccessStories, addSuccessStory, updateSuccessStory, deleteSuccessStory
- getTestimonials, addTestimonial, updateTestimonial, deleteTestimonial
- getBlogs, getBlogById, addBlog, updateBlog, deleteBlog
- getBranches, getBranchById, addBranch, updateBranch, deleteBranch
- getTeamMembers, addTeamMember, updateTeamMember, deleteTeamMember
- getVolunteers, addVolunteer, updateVolunteer
- getDonations, getDonationById

Cloud Functions in functions/index.js

- createRazorpayOrder using onCall to create a Razorpay order and pending donations record
- verifyRazorpayPayment using onCall to verify HMAC, complete the donation, create a donors record, and send a receipt
- razorpayWebhook using onRequest for webhook reconciliation
- sendVolunteerConfirmation using onCall to send volunteer confirmation mail

Other services

- src/services/firebase.js for Firebase initialization and shared exports
- src/services/cacheService.js for memory and localStorage caching
- src/services/emailService.js for EmailJS integration
- src/services/imageUploadService.js for Firebase Storage uploads
- src/services/razorpayService.js for client side Razorpay script and checkout helpers

## 3. Component inventory summary

Pages include HomePage, AboutPage, GalleryPage, TeamPage, MediaPage, EventsPage, BranchesPage, VolunteerPage, DonatePage, ContactPage, AdminLogin, AdminDashboard, MaintenancePage, and NotFoundPage.

Shared components include Button, FormInput, Modal, LoadingSpinner, Navbar, Footer, ProtectedRoute, ScrollToTop, SEO, ErrorBoundary, AnimatedCounter, ImageUpload, and AdminSetup.

Management components include BlogManagement, BranchManagement, DonationManagement, EventManagement, GalleryManagement, StoriesTestimonialsManagement, TeamManagement, and VolunteerManagement.

## 4. Environment variables

Client side values in .env

- VITE_FIREBASE_API_KEY required
- VITE_FIREBASE_AUTH_DOMAIN required
- VITE_FIREBASE_PROJECT_ID required
- VITE_FIREBASE_STORAGE_BUCKET required
- VITE_FIREBASE_MESSAGING_SENDER_ID required
- VITE_FIREBASE_APP_ID required
- VITE_FIREBASE_MEASUREMENT_ID optional
- VITE_FIREBASE_DATABASE_URL optional
- VITE_EMAILJS_SERVICE_ID optional when EmailJS is used
- VITE_EMAILJS_TEMPLATE_ID optional when EmailJS is used
- VITE_EMAILJS_PUBLIC_KEY optional when EmailJS is used
- VITE_RAZORPAY_KEY_ID required for Razorpay checkout

Server side values for Cloud Functions

- RAZORPAY_KEY_ID required
- RAZORPAY_KEY_SECRET required and server only
- RAZORPAY_WEBHOOK_SECRET required
- EMAIL_USER optional
- EMAIL_PASSWORD optional

Build and deployment commands

- npm run dev
- npm run build
- npm run preview
- firebase deploy
- firebase deploy --only hosting
- firebase deploy --only functions

## 5. Errors, debug, and security

Common error patterns

- Firestore permission denied usually means missing security rules or missing authentication for admin writes.
- React Query not refreshing after mutation usually means invalidateQueries was not called correctly.
- Razorpay checkout not opening usually means VITE_RAZORPAY_KEY_ID is missing or the SDK did not load.
- Email not sending usually means EmailJS configuration is missing or backend email credentials expired.
- Maintenance page showing unexpectedly usually means config/maintenanceMode is true.
- Firebase app not initializing usually means required VITE_FIREBASE values are missing.
- Stale data after admin action usually means query keys were not invalidated or cached data has not refreshed yet.

Security requirements

- Sanitize all user text before Firestore writes.
- Protect admin routes with ProtectedRoute.
- Keep RAZORPAY_KEY_SECRET only in Cloud Functions.
- Enforce Firestore and Storage write rules outside the client.
- Restrict volunteer and donor data reads to authorized users.

Debug approach

1. Check browser console for missing environment variable messages.
2. Check Firebase Console Firestore for data presence.
3. Check Firebase Console Functions logs for Cloud Function failures.
4. Inspect React Query state when data appears stale.
5. Review cache behavior and stale time values.

## 6. Utilities summary

- sanitization.js for input sanitization
- validators.js for validation schemas
- formatters.js and helpers.js for display formatting and helper logic
- logger.js for logging
- adminSetup.js for one time admin bootstrap support
