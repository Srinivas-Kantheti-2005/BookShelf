# Database Design: BookShelf v1

**Source:** docs/01-prd.md, docs/02-user-stories.md
**Database:** MongoDB (Mongoose)
**Date:** 6 Oct 2026

## 1. Collections
3 collections: User, Book, Note.

## 2. Relationships
- One User has many Books.
- One Book has many Notes.
- One User has many Notes.

```
User 1 ──── * Book 1 ──── * Note
  └─────────────────────────┘
        (User also owns Note)
```

## 3. User

| Field | Type | Rules |
|---|---|---|
| username | String | required, unique, trimmed, 3 to 30 chars |
| email | String | required, unique, lowercase, trimmed, valid email |
| password | String | required for email signup, optional for Google users, bcrypt hash, `select: false` |
| googleId | String | optional, unique sparse |
| isVerified | Boolean | default false, true after OTP or Google login |
| otpHash | String | optional, hashed 6 digit code, `select: false` |
| otpExpires | Date | optional, 10 minutes after sending |
| otpAttempts | Number | default 0, max 5 |
| otpLastSentAt | Date | optional, used for 60 second resend wait |
| createdAt | Date | auto |
| updatedAt | Date | auto |

## 4. Book

| Field | Type | Rules |
|---|---|---|
| title | String | required, trimmed, max 150 chars |
| author | String | required, trimmed, max 100 chars |
| genre | String | required, one of the fixed genre list (section 6) |
| description | String | optional, trimmed, max 2000 chars |
| status | String | required, one of: `want-to-read`, `reading`, `finished` |
| totalPages | Number | required, whole number, min 1 |
| currentPage | Number | default 0, whole number, min 0, max `totalPages` |
| cover.url | String | optional, Cloudinary URL |
| cover.filename | String | optional, Cloudinary id (needed to delete image) |
| startedAt | Date | optional, set when status becomes `reading` |
| finishedAt | Date | optional, set when status becomes `finished` |
| owner | ObjectId | required, ref User |
| createdAt | Date | auto |
| updatedAt | Date | auto |

Not stored: progress percent = `currentPage / totalPages * 100`. Calculated when shown.

## 5. Note

| Field | Type | Rules |
|---|---|---|
| text | String | required, trimmed, max 2000 chars |
| page | Number | optional, whole number, min 1, max book's `totalPages` |
| book | ObjectId | required, ref Book |
| owner | ObjectId | required, ref User |
| createdAt | Date | auto (shown on page) |
| updatedAt | Date | auto |

## 6. Fixed values

**Book status:** `want-to-read`, `reading`, `finished`

**Book genre:** `fiction`, `non-fiction`, `fantasy`, `sci-fi`, `mystery`, `thriller`, `romance`, `biography`, `history`, `self-help`, `science`, `technology`, `business`, `poetry`, `other`

## 7. Rules and behaviour

**Ownership**
- Every Book and Note stores its owner. Every query filters by owner so users never see each other's data.
- `owner` and `book` are set by the server, never taken from the form.

**Book status and pages**
- `want-to-read`: `currentPage` is 0, `startedAt` and `finishedAt` are cleared.
- `reading`: `currentPage` is between 1 and `totalPages`, `startedAt` is set if empty, `finishedAt` is cleared.
- `finished`: `currentPage` equals `totalPages`, `finishedAt` is set.
- `currentPage` over `totalPages` is rejected.

**Notes**
- Note `page` over the book's `totalPages` is rejected (controller loads the book and checks).
- A note can only be added to a book the user owns.
- Edit changes only `text` and `page`, never `book` or `owner`.
- Notes show newest first. A note shows "edited" when `updatedAt` is later than `createdAt`.

**Delete**
- Deleting a Book deletes all its Notes.
- Deleting a Book also deletes its cover image on Cloudinary.

**Auth data**
- Passwords are hashed before saving, never stored or logged in plain text.
- `password` and `otpHash` are hidden from queries by default. Login code asks for them on purpose.
- OTP codes are stored hashed and cleared after successful verification.
- Unverified users cannot log in.
- Register with an email that exists but is unverified: username and password are overwritten and a new OTP is sent. If the email is verified, it is rejected as duplicate.
- Google users are verified automatically and may have no password.
- Google username is made from the Google name, lowercase, no spaces, plus 4 random digits, checked for uniqueness.
- Account linking: if the Google email matches an existing user and Google reports the email as verified, set `googleId` on that user. Otherwise do not link.

## 8. Indexes
- User: unique index on `username`, unique index on `email`.
- User: unique sparse index on `googleId`.
- Book: index on `owner` (library list).
- Book: index on `owner + status` (filter by status).
- Book: index on `owner + genre` (filter by genre).
- Book: text index on `title` and `author` (search).
- Note: index on `book + createdAt` descending (notes of one book, newest first).

## 9. Validation layers
1. Client: JavaScript form validation.
2. Server: Joi schemas on request body.
3. Database: Mongoose schema rules.

## 10. Decisions
- Notes are a separate collection (not an array inside Book), so each note can be edited and deleted alone.
- Note stores `owner` even though Book knows its owner, so edit and delete need only one query.
- Genre is a fixed list, not free text, so filter works cleanly.
- Cover stored as object (`url` + `filename`) so image can be removed from Cloudinary later.
- Progress percent is calculated, not stored, so it never goes stale.
- JWT is stateless, so no session or token collection. "Remember me" only changes token and cookie lifetime.
- OTP fields live inside User (one active code per user), no separate collection.
- Reviews and ratings are not in v1. Personal notes only.