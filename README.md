# Family Sathi — MVP starter

Responsive Hindi-first React + Vite website with Firebase Authentication, Firestore-backed course loading, registration/login, enrollments, and a server-enforced admin area.

## What works after Firebase setup
- Email/password registration and login
- Email verification request after registration
- User profile stored in Firestore (password is never stored in Firestore)
- Published course listing and course enrollment
- Admin-only course publishing, enforced by Firestore Security Rules
- Responsive home, courses, dashboard, about, and admin screens

## Important current limits
- This project is a starter, not a published website. Firebase config is intentionally a placeholder.
- Starter courses in `src/main.jsx` are demo content until you add published course documents to Firestore.
- The UI is not a substitute for reviewing security rules, privacy/consent text, accessibility, and production readiness before public launch.
- The site uses a Google Fonts import for typography; if offline, system fallback fonts are used.

## Setup (free tier to start)
1. Create a project at https://console.firebase.google.com/.
2. In **Build → Authentication → Get started → Sign-in method**, enable **Email/Password**.
3. In **Project settings → Your apps**, add a Web app and copy its config into `src/firebase.js`, replacing each `YOUR_...` placeholder.
4. In **Firestore Database**, create the database.
5. Publish the contents of `firestore.rules` in Firestore → Rules. Review the rules before production.
6. Create the first admin safely:
   - Register your own account using the site.
   - In Firebase Console → Firestore, create collection `admins` and a document whose document ID is your Firebase Authentication user's exact UID. Example fields: `{ "createdAt": "manual-bootstrap" }`.
   - Only do this for an account you control. Never let members create or edit documents in `admins`.
   - The rule permits later admin management only for an existing admin. Keep console access protected with strong account security.
7. Add course documents to collection `courses`. Each document should include `title`, `category`, `level`, `duration`, `description`, and `published: true`.
8. Install Node.js LTS, then run:
   ```bash
   npm install
   npm run dev
   ```
9. For a production build, run `npm run build`. Deploy the `dist` directory to Firebase Hosting or another static host. Configure your domain and hosting rewrites as needed.

## Admin security model
- Admin status is checked against the existence of `admins/{Firebase UID}` in Firestore, not a browser-only flag.
- Firestore Rules deny member writes to `admins`, courses, and other users' records.
- The first admin document must be created by a trusted project owner through the Firebase Console. Do not put a shared admin password in source code.
- Treat Firebase web config as public client configuration; database security comes from Authentication and Firestore Rules, not from hiding the API key.

## Before public launch
- Add and publish a real privacy policy, terms, contact/support information, data deletion process, and age/guardian rules if children can register.
- Review applicable Indian privacy and child-safety obligations.
- Set Firebase Authentication email action URL/domain settings for the production domain.
- Test member vs admin access using Firebase Rules emulator or separate test accounts.
- Configure backups/export and test recovery.
- Add password reset, abuse reporting, rate limiting/App Check where appropriate, and monitoring.
