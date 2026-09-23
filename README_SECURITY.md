# DEEB CHOCOLATE — Admin OS

## Included
- `index.html` — current DEEB Chocolate Admin OS.
- `firestore.rules` — Firebase Firestore rules; all business data is Admin-only.

## Admin bootstrap
Create Firestore collection `admins` and a document whose ID is the Firebase Auth UID of the owner.
Fields:
- `active`: boolean `true`
- `role`: string `owner`

The current owner UID was configured manually in Firebase during setup.

## Security notes
- The frontend Firebase web configuration is not a private service-account credential.
- Never commit Firebase service-account JSON files, private keys, passwords, `.env` secrets, or customer/order exports.
- `activityLog` can be read/created by active admins but cannot be updated/deleted by Firestore rules.
- The current public customer order form was intentionally omitted from the final build because DEEB's official website already handles customer ordering.
- Weekly email backup requires a backend/Cloud Function or an approved mail service; the browser-only app cannot safely send scheduled emails by itself.
- Manual JSON backup and restore are available from Settings. Excel import is client-side and admin-only.
