# DEEB CHOCOLATE — Admin OS Release

This release contains the current DEEB Chocolate Admin OS and its Firestore security rules.

## Files

- `index.html` — main DEEB admin dashboard.
- `order-form.html` — customer-facing order form using the same Firebase project.
- `firestore.rules` — Firebase access-control rules.

## Firebase setup

The dashboard requires Firebase Authentication and the `admins/{UID}` document for each admin account.

For the owner account, the document contains:

- `active: true`
- `role: owner`

The Firestore rules allow only active admins to access private collections.

The customer order form can create only `pending` orders with `source: customer-form`. Product and flavor documents are readable publicly because the customer form needs them to build its dropdowns; writes remain admin-only.

## ERP collections added

- `suppliers`
- `supplierOrders`
- `supplierPriceHistory`
- `stockMovements`

## Important

Do **not** commit passwords, service-account JSON files, private keys, `.env` secrets, customer exports, or database backups to GitHub.

Manual JSON backup/import is available from Settings. A truly automatic weekly email backup requires a trusted server-side scheduler/email service; it is not performed directly from the browser in this release.
