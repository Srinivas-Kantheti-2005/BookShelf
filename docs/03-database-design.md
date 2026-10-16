# Database Design: BookShelf v1

**Source:** docs/01-prd.md, docs/02-user-stories.md
**Database:** MongoDB (Mongoose)
**Date:** 16 Oct 2026

## 1. Overview

Three models hold the app data: User, Book, and Note. One more collection, `sessions`, is created and cleaned by a library (`connect-mongo`) and holds only temporary data.

| Collection | Kind | Holds |
|---|---|---|
| `users` | Model | Accounts, login data, profile image |
| `books` | Model | A reader's books |
| `notes` | Model | A reader's notes on a book |
| `sessions` | Managed by `connect-mongo` | Flash messages, Google OAuth state, the pending email for verify and reset. Records expire on their own (TTL). It never holds login state |

Images are not stored in the database. Covers and avatars live on Cloudinary, and the database keeps their URL and id.

## 2. Relationships

- One User has many Books.
- One Book has many Notes.
- One User has many Notes.

```
User 1 ──── * Book 1 ──── * Note
  └─────────────────────────┘
        (User also owns Note)
```

## 3. Fixed values and limits

| Item | Value |
|---|---|
| Book status | `want-to-read`, `reading`, `finished` |
| Book genre | `fiction`, `non-fiction`, `fantasy`, `sci-fi`, `mystery`, `thriller`, `romance`, `biography`, `history`, `self-help`, `science`, `technology`, `business`, `poetry`, `other` |
| Code purpose | `verify-email`, `reset-password` |
| Username | 3 to 30 characters, lowercase letters, numbers, underscore |
| Password | at least 8 characters, with a letter and a number |
| Code (OTP) | 6 digits, expires after 10 minutes, 5 wrong attempts, 60 seconds between sends |
| Unverified account | deleted after 24 hours |
| Library page size | 12 books |
| Image upload | JPG, PNG, or WEBP, up to 2 MB |
| Cloudinary folders | `bookshelf/covers`, `bookshelf/avatars` |
| Text limits | title 150, author 100, book description 2000, note text 2000 |
| Session length | Remember me off: until the browser closes. On: 30 days |

## 4. User

| Field | Type | Rules |
|---|---|---|
| username | String | required, unique, stored lowercase, 3 to 30 characters, letters, numbers, and underscore only |
| email | String | required, unique, lowercase, trimmed, valid email |
| password | String | bcrypt hash, hidden from queries by default (`select: false`). Missing for a Google-only account |
| googleId | String | optional, unique (sparse). Set for Google users |
| isVerified | Boolean | default `false`. `true` after the code is entered, after a password reset, or after Google sign up |
| otpHash | String | optional, hashed 6 digit code, hidden from queries by default |
| otpPurpose | String | optional, `verify-email` or `reset-password` |
| otpExpires | Date | optional, 10 minutes after the code is sent |
| otpAttempts | Number | default 0, at most 5 |
| otpLastSentAt | Date | optional, used for the 60 second wait |
| passwordChangedAt | Date | optional, set when a password is changed, set, or reset |
| avatar.url | String | optional, Cloudinary URL (uploaded by the user, or copied from Google) |
| avatar.filename | String | optional, Cloudinary id, needed to delete the image |
| createdAt | Date | automatic. Shown as "member since" |
| updatedAt | Date | automatic |

## 5. Book

| Field | Type | Rules |
|---|---|---|
| title | String | required, trimmed, max 150 |
| author | String | required, trimmed, max 100 |
| genre | String | required, one value of the genre list |
| description | String | optional, trimmed, max 2000 |
| status | String | required, one of `want-to-read`, `reading`, `finished` |
| totalPages | Number | required, whole number, at least 1 |
| currentPage | Number | default 0, whole number, at least 0, at most `totalPages` |
| cover.url | String | optional, Cloudinary URL |
| cover.filename | String | optional, Cloudinary id, needed to delete the image |
| startedAt | Date | optional, set when the status becomes `reading` |
| finishedAt | Date | optional, set when the status becomes `finished` |
| owner | ObjectId | required, ref User |
| createdAt | Date | automatic. Used for the library order |
| updatedAt | Date | automatic |

## 6. Note

| Field | Type | Rules |
|---|---|---|
| text | String | required, trimmed, max 2000 |
| page | Number | optional, whole number, at least 1, at most the book's `totalPages` |
| book | ObjectId | required, ref Book |
| owner | ObjectId | required, ref User |
| createdAt | Date | automatic. Shown on the note |
| updatedAt | Date | automatic. A note is "edited" when it is later than `createdAt` |

## 7. Sessions (managed by `connect-mongo`)

| Data | Used for |
|---|---|
| Flash messages | A message shown once on the next page |
| Google OAuth `state` | Safety check of the Google login |
| Pending email (email, purpose, expiry of 15 minutes) | The verify page and the reset page, so the email is not in the URL |

The collection expires its own records. The login itself is the JWT cookie, so no login data is stored here.

## 8. Calculated values (never stored)

| Value | How | Why not stored |
|---|---|---|
| Progress percent | `currentPage / totalPages * 100` | It would go stale |
| Sign-in method | `password` set means Password, `googleId` set means Google, both means Password and Google | It follows from the fields |
| Profile numbers | Total books, books per status, pages read (sum of `currentPage`), total notes, all from queries on the owner's data | They would go stale |
| Library total and page count | A count query with the same filter as the list | They change with every filter |

## 9. Rules and behaviour

### 9.1 Ownership
- Every Book and Note stores its `owner`. Every query on books and notes includes `owner`, so a user never sees another user's data.
- `owner` and `book` are set by the server from the logged-in user and the URL, never from the form.
- A note is only created, read, changed, or deleted when it belongs to both the user and the book in the URL.

### 9.2 Book status, pages, and dates
- `want-to-read`: `currentPage` is 0. `startedAt` and `finishedAt` are cleared.
- `reading`: `currentPage` is between 1 and `totalPages`. `startedAt` is set if empty. `finishedAt` is cleared.
- `finished`: `currentPage` equals `totalPages`. `finishedAt` is set. `startedAt` is kept if it exists.
- A `currentPage` above `totalPages` is rejected.
- Lowering `totalPages` below `currentPage` is rejected.

### 9.3 Notes
- A note's `page` above the book's `totalPages` is rejected. The controller loads the book and checks, because a model cannot see another document.
- Editing changes only `text` and `page`, never `book` or `owner`.
- Notes show newest first.

### 9.4 Delete and cleanup
- Deleting a Book deletes all its Notes, and deletes its cover on Cloudinary.
- Replacing or removing a cover or an avatar deletes the old file on Cloudinary.
- A daily job deletes users who are not verified after 24 hours, together with their avatar on Cloudinary. Unverified users have no books or notes.
- A username held by an unverified account stays taken until that account is deleted.

### 9.5 Passwords and sessions
- A password is hashed with bcrypt before saving. It is never stored or logged in plain text.
- `password` and `otpHash` are hidden from queries. Login and verify code ask for them on purpose.
- A JWT issued before `passwordChangedAt` is rejected. Compare in whole seconds, so a token issued in the same second is valid.
- Change password checks the current password with bcrypt, saves the new hash, sets `passwordChangedAt`, and issues a new JWT for the current session.
- Reset password saves the new hash, sets `passwordChangedAt`, clears the code, and sets `isVerified` to `true`. All sessions end.
- A Google-only account (no password) can set a password without a current one.

### 9.6 Codes (OTP)
- A 6 digit code is stored hashed in `otpHash`, with `otpPurpose`, `otpExpires`, `otpAttempts`, and `otpLastSentAt`.
- A new code replaces any older code, so only one code is active per user.
- A wrong code adds 1 to `otpAttempts`. At 5 the code is dead and a new one is needed.
- A code only works for its own purpose.
- After success, all code fields are cleared.
- Unverified users cannot log in.
- Registering again with an email that is unverified replaces the username, the password, and the avatar (the old file is deleted), and sends a new code. An email that belongs to a verified account is rejected.
- Forgot password sends no code to an account without a password.

### 9.7 Google accounts
- A new Google user is verified at once and has no password.
- The username is made from the Google name: lowercase, only allowed characters, no spaces, plus 4 random digits, checked for uniqueness, and cut so the total stays within 30 characters.
- A Google email that matches an existing account is linked (`googleId` is set) only when Google reports the email as verified. Otherwise the accounts are not linked.
- The Google photo is copied to Cloudinary once, only when the account has no avatar. If the fetch fails, the account is still created.

### 9.8 Library queries
- Filter: `owner`, plus `status` and `genre` when set, plus search.
- Search: a case-insensitive match on `title` or `author`, with the user's text escaped so it cannot act as a pattern.
- Order: `createdAt` newest first. Page size 12, using skip and limit.
- A page number that is not valid becomes 1. A page number above the last page becomes the last page.

## 10. Indexes

| Collection | Index | Purpose |
|---|---|---|
| users | unique `username` | no duplicates, login lookup |
| users | unique `email` | no duplicates, forgot password lookup |
| users | unique sparse `googleId` | Google login lookup |
| users | `isVerified` + `createdAt` | cleanup of unverified accounts |
| books | `owner` + `createdAt` descending | the library list, newest first |
| books | `owner` + `status` + `createdAt` descending | the status filter |
| books | `owner` + `genre` + `createdAt` descending | the genre filter |
| notes | `book` + `createdAt` descending | notes of one book, newest first |
| notes | `owner` | the note count on the profile |
| sessions | expiry (TTL) | created and managed by `connect-mongo` |

## 11. Validation layers

1. Client: JavaScript form checks, with errors that clear when the user types.
2. Server: Joi schemas check every request body, query, and file.
3. Database: Mongoose schema rules and indexes.

None of the three replaces the others.

## 12. Decisions

- Notes are a separate collection (not an array inside Book), so each note can be edited and deleted alone.
- A Note stores `owner` even though its Book knows its owner, so edit and delete need one query.
- Genre is a fixed list, not free text, so the filter works cleanly.
- Covers and avatars are stored as `url` + `filename`, so the file can be deleted on Cloudinary later.
- Progress, profile numbers, and sign-in method are calculated, so they never go stale.
- Search uses a case-insensitive match limited to the owner's books, because a text index only finds whole words and search runs as the user types. A single reader's library is small, so this is fast.
- JWT stays stateless. `passwordChangedAt` is the only server-side check that ends old sessions.
- The `sessions` collection holds temporary data only. It is separate from login.
- OTP fields live inside User (one active code per user), with no separate collection.
- The Google photo is copied, not linked, so it does not break when the user changes their Google photo.
- Reviews and ratings are not in v1. Personal notes only.

## 13. Requirements covered

| Area | Requirements | Where |
|---|---|---|
| Register, verify, cleanup | FR-4 to FR-12 | User fields, 9.4, 9.6 |
| Login, session | FR-13 to FR-18 | User, 9.5 |
| Forgot and reset | FR-19 to FR-23 | User code fields, 9.5, 9.6 |
| Google | FR-24 to FR-28 | `googleId`, 9.7 |
| Library | FR-29 to FR-37 | Book, 9.8, indexes |
| Books and cover | FR-38 to FR-50 | Book, 9.2, 9.4 |
| Notes | FR-51 to FR-54 | Note, 9.3 |
| Profile | FR-55 to FR-65 | User, section 8, 9.5 |