# SplitMate

A mobile-friendly shared expense splitting website built with HTML, CSS, JavaScript modules and Firebase Authentication + Cloud Firestore.

## Features
- Create categories such as Party, Trip or Birthday.
- Invite people with a category link.
- Real-time expense updates across devices.
- Record payer, description and amount in INR.
- Calculate suggested transfers from expenses and group members.

## Required setup before live use

### 1. Configure Firebase
1. Open https://console.firebase.google.com/ and create a project.
2. Register a Web app and copy its Firebase config.
3. In `index.html`, replace the four placeholder values in `firebaseConfig` with the exact values from Firebase:
   - `YOUR_FIREBASE_API_KEY` → `apiKey`
   - `YOUR_FIREBASE_PROJECT` → the project name in `authDomain` (for example, `my-project.firebaseapp.com`)
   - `YOUR_FIREBASE_PROJECT_ID` → `projectId`
   - `YOUR_FIREBASE_APP_ID` → `appId`
4. In Firebase Console → Build → Authentication → Sign-in method, enable **Anonymous**.
5. In Firebase Console → Build → Firestore Database, create a database.

### 2. Publish Firestore security rules
1. Open Firestore Database → Rules.
2. Replace the rules with the full contents of `firestore.rules`.
3. Click **Publish**.

The app is intended for groups where anyone holding the invite link can join. Treat invite links as shareable access links. The group document contains the invite code; people who know a group's ID can read its metadata to validate joining. Do not store sensitive information in groups. Never commit service-account private keys or passwords.

### 3. Deploy for free
1. Sign in at https://dash.cloudflare.com/
2. Open Workers & Pages → Create → Pages → Connect to Git.
3. Connect GitHub and choose `SplitMate`.
4. Framework preset: **None**. Leave the build command blank. Set the build output directory to `/` (the repository root, if accepted by the interface).
5. Deploy. Cloudflare will provide a public `*.pages.dev` URL.

After Firebase is configured and the site is deployed, open the URL and create a category to test it. Open its invite link in another browser/device to check shared updates.

## Current MVP limitations
- Expense split calculations assume an equal share among all group members; the current split selector is basic and should be treated as a prototype.
- Settlement suggestions are calculated; no payments are processed.
- Expense edit/delete and settlement confirmation are not implemented yet.
- Display names are user-entered and are not verified identities.
- This is an MVP, not a payment system or audited financial ledger.
