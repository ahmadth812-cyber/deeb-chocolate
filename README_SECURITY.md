# DEEB CHOCOLATE — Firebase Security Setup

## 1. Deploy the rules

Upload `firestore.rules` to the Firebase project used by the DEEB dashboard.

Firebase project:
- Project ID: `deeb-chocolate`

## 2. Create the first admin

The new rules do NOT trust `request.auth != null` by itself.

In Firebase Console:

1. Open Authentication → Users.
2. Find the DEEB owner/admin account.
3. Copy that user's **UID**.
4. Open Firestore Database.
5. Create collection: `admins`
6. Create a document whose **Document ID is exactly the Firebase Auth UID**.
7. Add:
   - `active` → boolean → `true`
   - `role` → string → `owner`

Example:

```text
admins/
  xxxxxxxxxxxxxxxxx
    active: true
    role: "owner"
```

## 3. Adding another admin

Create another `/admins/{theirAuthUid}` document manually with:

```text
active: true
role: "admin"
```

Do not give the dashboard permission to create or edit documents in `admins`.

## 4. Removing an admin

Set their `active` field to `false`, or delete their `/admins/{uid}` document from Firebase Console.

## 5. Activity log

`activityLog` is append-only from the dashboard:

- read: allowed for active admins
- create: allowed for active admins
- update: denied
- delete: denied

This prevents the browser dashboard from rewriting or deleting audit history.

## 6. Why this is safer than the old rules

Old behavior:

```text
Any authenticated Firebase user
        ↓
read/write every business collection
```

New behavior:

```text
Firebase Auth user
        ↓
Must exist in /admins/{uid}
        ↓
active == true
        ↓
DEEB data access
```

A normal authenticated account cannot grant itself admin access because the `/admins` collection is not client-writable.

## 7. Important production note

The Firebase Web API key inside the frontend is not a password. Firebase web apps normally expose this configuration to the browser. Security must come from Authentication, Firestore Rules, Firebase/Google API restrictions, and appropriate App Check configuration.

Do NOT commit private service-account JSON files, private keys, passwords, or server credentials to GitHub.
