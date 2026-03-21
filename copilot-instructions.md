Copilot Instructions for Dada Chi Shala Website

Purpose: This file defines how agents should work in this repository. Domain knowledge lives in the other knowledge base files. This file governs behavior.
Organization: Educare Dada Chi Shala Educational Trust
Last updated: 2026-03-21

1. Knowledge base navigation

Repository file map

- README.md for the project overview and starting order.
- INDEX.md for keyword lookup, routing, and module selection.
- 01-public-storytelling-module.md for public storytelling pages.
- 02-events-module.md for events.
- 03-community-engagement-module.md for volunteers and branches.
- 04-donations-fundraising-module.md for donations and payment flows.
- 05-admin-content-management-module.md for admin dashboard behavior.
- 06-platform-infrastructure-auth-module.md for auth, routing, Firebase setup, and shared services.
- qc_quick_reference.md for collection fields, hooks, services, environment variables, and common issues.
- ARCHITECTURE.md for cross system context.

Mandatory retrieval protocol

1. Read INDEX.md first.
2. Read the relevant module file.
3. Cross check qc_quick_reference.md before making technical claims about fields, hooks, services, or environment variables.
4. If the topic is not covered, say that clearly instead of guessing.

File priority

1. INDEX.md on every interaction.
2. The target module file for the active feature.
3. qc_quick_reference.md for exact technical lookup.
4. ARCHITECTURE.md when system context or cross module behavior matters.

2. Mandatory rules

KB first and no guessing

- Every technical claim about this codebase must be backed by a knowledge base file.
- If the knowledge base does not cover a topic, say so explicitly.
- Look up collection names, field names, component names, hook names, and service methods before using them.

Anti hallucination rules

- Firestore collection names must exist in qc_quick_reference.md or be clearly marked as new.
- Firestore field names must exist in the data model or be clearly marked as proposed.
- Hook names must match the documented useFirebaseQueries exports.
- Service methods must match the documented cachedDatabaseService API.
- Route paths must match INDEX.md and ARCHITECTURE.md.
- Cloud Function names must come from the donations or community modules.
- Component file names must match the actual project structure.

Source marking

When presenting technical guidance, cite the source in plain language when practical, such as the relevant module, quick reference section, architecture document, user provided context, or a clearly marked proposed item.

Flag gaps immediately

If a topic is not covered in the knowledge base, state that the documentation is missing for that topic and that manual validation is required.

React and Firebase conventions

- Firestore reads must use React Query hooks, not raw Firestore SDK calls in component bodies.
- Mutations must invalidate related query keys after success.
- User input must be sanitized before writing to Firestore.
- Admin routes must use ProtectedRoute.
- Cloud Function secrets must stay server side.
- Client environment variables must use the VITE prefix.

3. Project summary

This is a React and Firebase single page application for an NGO called Dada Chi Shala.

The site provides:

- A public portal for awareness, storytelling, gallery, team, events, and media.
- A volunteer recruitment flow that stores data in Firestore and supports admin review.
- A donation flow with Razorpay online payment and manual transfer handling.
- An admin dashboard for content CRUD operations.

Key architecture choices

- One SPA handles public and admin experiences.
- Firebase is the main infrastructure dependency.
- React Query is the data layer for reads.
- cachedDatabaseService is the write abstraction.
- cacheService provides memory and localStorage caching.
- Maintenance mode is controlled through config/maintenanceMode in Realtime Database.

4. Code quality standards

- Security: never expose secrets client side, sanitize all user input, and enforce admin auth.
- Data writes: always go through cachedDatabaseService.js.
- Data reads: always go through useFirebaseQueries.js hooks.
- Forms: use react-hook-form and yup.
- Styling: match the existing Tailwind CSS patterns.
- Mutations: invalidate relevant query keys after success.
- Errors: surface errors in UI state and do not let email failures block successful writes.
- Code splitting: new pages should use React.lazy and Suspense when they fit the route model.
- Naming: components in PascalCase, hooks in useXxx form, services in camelCase.
- Comments: add comments only where logic is not obvious.

5. Output standards

- Keep generated documents plain and readable.
- Preserve Mermaid diagrams where they help explain flow.
- List documentation gaps clearly.
- Mark newly proposed items clearly.
- Follow the project naming conventions for files, components, hooks, and collections.

6. Escalation protocol

Stop and tell the user when:

1. The topic has no knowledge base coverage.
2. Two knowledge base files conflict.
3. The request depends on infrastructure or integrations not documented here.
4. The request would expose secrets, bypass auth, or skip input validation.
