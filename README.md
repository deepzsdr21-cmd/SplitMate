# SplitMate

A mobile-friendly shared expense splitting website built with HTML, CSS, JavaScript modules and Firebase Authentication + Cloud Firestore.

## Features
- Create categories such as Party, Trip or Birthday.
- Invite people with a category link.
- Real-time expense updates across devices.
- Record payer, description and amount in INR.
- Calculate suggested transfers from expenses and group members.

## Firebase backend

The Firebase project for SplitMate is `splitmate-8ce1a`. It is on the Spark plan, with no billing account linked. The `SplitMate Web` app is registered, and its web SDK configuration is in `index.html`. Anonymous Authentication is enabled. Google Analytics and Gemini in Firebase are disabled.

Cloud Firestore uses the Standard edition and default database in Mumbai (`asia-south1`). Firebase does not let you change the database location after creation. The project remains on the Spark plan; it will not upgrade automatically to paid billing.

### Firestore security rules

The rules are in `firestore.rules`. They allow a signed-in user to read their own membership record before joining, validate the invite code when creating membership, and allow access to group data only after membership is established. Expenses are readable and creatable by members; unsupported expense edits and deletes are denied.

To publish the rules from a signed-in Firebase CLI session:

```sh
npx -y firebase-tools@latest deploy --only firestore:rules
```

The `.firebaserc` file selects `splitmate-8ce1a`, and `firebase.json` points the CLI to the repository rules.

Group invite links grant access to that group's shared ledger. Share them only with intended participants, and do not store sensitive information in groups. Never commit service-account private keys or passwords. The web API key in `index.html` is a public Firebase client identifier; authorization is enforced by Firebase Authentication and Firestore rules.

### Free-plan usage

Cloud Firestore Standard includes a free daily quota of 50,000 reads, 20,000 writes, and 20,000 deletes, plus 1 GiB stored data and 10 GiB monthly outbound transfer. On Spark, exceeding a no-cost quota shuts that product off for the rest of the billing cycle rather than charging overage. Anonymous Authentication is available on Spark at no cost. See [Firebase pricing plans](https://firebase.google.com/docs/projects/billing/firebase-pricing-plans) and [Cloud Firestore pricing](https://firebase.google.com/docs/firestore/pricing).

## Current MVP limitations
- Expense split calculations assume an equal share among all group members; the current split selector is basic and should be treated as a prototype.
- Settlement suggestions are calculated; no payments are processed.
- Expense edit/delete and settlement confirmation are not implemented yet.
- Display names are user-entered and are not verified identities.
- This is an MVP, not a payment system or audited financial ledger.
