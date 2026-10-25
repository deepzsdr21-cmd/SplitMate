# SplitMate

A mobile-friendly shared expense splitting website built with HTML, CSS, JavaScript modules and Firebase Authentication + Cloud Firestore.

## Features
- Create categories such as Party, Trip or Birthday.
- Invite people with a category link.
- Real-time expense updates across devices.
- Create a category without assigning one person as its owner.
- Each participant can join with their own name, and any joined member can add a named participant who is unavailable to join.
- Expenses can be recorded under any participant's name, including a ₹0 entry to mark no spend; their expense status remains visible.
- Participants record their own expenses; new expenses start split equally across everyone already joined.
- Adjust the people sharing any expense later from its row in the Expenses tab, including participants who joined afterward and have spent nothing.
- Calculate who pays whom and how much from each expense's selected participants.
- Category creators can permanently delete their category and its expenses and participant records.
- Calculate who pays whom and how much from each expense's selected participants.

## Firebase backend

The Firebase project for SplitMate is `splitmate-8ce1a`. It is on the Spark plan, with no billing account linked. The `SplitMate Web` app is registered, and its web SDK configuration is in `index.html`. Anonymous Authentication is enabled. Google Analytics and Gemini in Firebase are disabled.

Cloud Firestore uses the Standard edition and default database in Mumbai (`asia-south1`). Firebase does not let you change the database location after creation. The project remains on the Spark plan; it will not upgrade automatically to paid billing.

### Firestore security rules

The rules are in `firestore.rules`. They allow a signed-in user to read their own membership record before joining, validate the invite code when creating membership, and allow access to group data only after membership is established. Expenses are readable and creatable by category members; a new expense is tied to the authenticated participant who paid it, and unsupported expense edits and deletes are denied.

To publish the rules from a signed-in Firebase CLI session:

```sh
npx -y firebase-tools@latest deploy --only firestore:rules
```

The `.firebaserc` file selects `splitmate-8ce1a`, and `firebase.json` points the CLI to the repository rules.

Group invite links grant access to that group's shared ledger. Share them only with intended participants, and do not store sensitive information in groups. Never commit service-account private keys or passwords. The web API key in `index.html` is a public Firebase client identifier; authorization is enforced by Firebase Authentication and Firestore rules.

### Free-plan usage

Cloud Firestore Standard includes a free daily quota of 50,000 reads, 20,000 writes, and 20,000 deletes, plus 1 GiB stored data and 10 GiB monthly outbound transfer. On Spark, exceeding a no-cost quota shuts that product off for the rest of the billing cycle rather than charging overage. Anonymous Authentication is available on Spark at no cost. See [Firebase pricing plans](https://firebase.google.com/docs/projects/billing/firebase-pricing-plans) and [Cloud Firestore pricing](https://firebase.google.com/docs/firestore/pricing).

## Deploy the website

The site is a static HTML app and can be hosted with Firebase Hosting on the existing Spark plan. Firebase Hosting includes up to 10 GB of stored site files and 10 GB of monthly data transfer at no cost. On Spark, exceeding a free quota disables Hosting until usage resets; the project will not be charged or upgraded automatically. See [Firebase Hosting quotas](https://firebase.google.com/docs/hosting/usage-quotas-pricing) and [Firebase pricing plans](https://firebase.google.com/docs/projects/billing/firebase-pricing-plans).

The repository's `firebase.json` configures Hosting to serve the site from the repository root while excluding the Firebase rules and documentation. The `.firebaserc` file selects project `splitmate-8ce1a`.

To deploy from a terminal with Firebase CLI installed and signed in:

```sh
npx -y firebase-tools@latest login
npx -y firebase-tools@latest deploy --only hosting
```

The live site will be available at [https://splitmate-8ce1a.web.app](https://splitmate-8ce1a.web.app) after a successful deployment. Future deployments use the same command after pulling updates from `main`.

After deployment, open the URL and create a category. Share its invite link with participants, then add an expense to confirm the live updates.

## Current MVP limitations
- Each expense is split equally among the people selected for that expense. New expenses default to everyone who has joined when they are added; adjust the selection in Expenses afterward.
- Participant names are user-entered display names tied to anonymous browser accounts; they are not verified identities.
- Settlement suggestions are calculated; no payments are processed.
- Any category member can edit an expense's payer, amount, description, or split; expense deletion and settlement confirmation are not implemented.
- This is an MVP, not a payment system or audited financial ledger.
