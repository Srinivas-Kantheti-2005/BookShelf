# Routes: BookShelf v1

**Source:** docs/01-prd.md, docs/02-user-stories.md, docs/03-database-design.md
**Date:** 16 Oct 2026

## 1. Rules

- Pages are server-rendered EJS. HTML forms send `GET` or `POST` only.
- `PUT` and `DELETE` use `method-override` (`?_method=PUT`, `?_method=DELETE`).
- Logout is `POST`, so a link or an image cannot log a user out.
- After a `POST`, `PUT`, or `DELETE` the server redirects with status 303, so the browser loads the next page with `GET`.
- Every `:id` route also checks that the record belongs to the logged-in user. Not found, or not yours, shows the 404 page.
- `owner`, `book`, and ids are never read from the form. The server sets them.
- Forms with a file (register, books, profile edit) use `multipart/form-data`.
- State-changing requests are only `POST`, `PUT`, or `DELETE`. Cookies are `sameSite: lax`. CSRF tokens are a later hardening step.
- A route that is not listed here shows the 404 page.

### 1.1 Access levels

| Level | Meaning | If the rule is not met |
|---|---|---|
| Public | Anyone | |
| Guest | Visitors only | A logged-in user is sent to `/books` |
| Pending | A visitor with a pending email in the session for this flow | The visitor is sent to the first page of the flow |
| Login | A valid JWT cookie, a verified account, and a token not older than `passwordChangedAt` | The visitor is sent to `/` with a message |

### 1.2 Cookies and session

| Item | Value |
|---|---|
| JWT cookie `bs_token` | `httpOnly`, `sameSite: lax`, `secure` in production. Holds the user id and the issue time |
| Remember me off | Cookie without an expiry (ends when the browser closes). The JWT expires after 1 day |
| Remember me on | Cookie and JWT last 30 days |
| Session cookie `bs_sid` | `httpOnly`, `sameSite: lax`, `secure` in production. Holds only temporary data |
| Pending email | In the session: email, purpose (`verify-email` or `reset-password`), expiry of 15 minutes. Never in the URL |

## 2. Landing page

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/` | Public | Landing page. A logged-in user is sent to `/books` | US-1 |

## 3. Authentication routes

### 3.1 Register and verify email

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/register` | Guest | Register form with the optional photo | US-2 |
| POST | `/register` | Guest | Validate. Upload the photo if sent (a photo from a failed form is deleted). Create the unverified user, or replace an earlier unverified attempt with the same email. Send a code, save the pending email, redirect to `/verify-email` | US-2 |
| GET | `/verify-email` | Pending (`verify-email`) | Six box code form. Without a pending email, redirect to `/register` | US-3, US-4 |
| POST | `/verify-email` | Pending (`verify-email`) | Check the code. Success: mark verified, clear the code, set the JWT, redirect to `/books`. Wrong code: show the form again with an error | US-3 |
| POST | `/verify-email/resend` | Pending (`verify-email`) | New code, 60 seconds after the last one, redirect to `/verify-email` with a message | US-4 |

### 3.2 Login and session

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/login` | Guest | Login form with Remember me, Forgot password, Google, and Register links | US-6 |
| POST | `/login` | Guest | Check username and password. Success: set the JWT cookie and redirect to `/books`. An unverified user gets a pending email and is sent to `/verify-email`. Wrong credentials show one generic error | US-6, US-7 |
| POST | `/logout` | Login | Clear the cookie, redirect to `/` with a message | US-8 |

### 3.3 Forgot and reset password

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/forgot-password` | Guest | Email form | US-10 |
| POST | `/forgot-password` | Guest | Always the same message. An account with a password gets a code and a pending email. A Google-only account gets an email about Google login. Redirect to `/reset-password` | US-10 |
| GET | `/reset-password` | Pending (`reset-password`) | Code and new password form. Without a pending email, redirect to `/forgot-password` | US-11 |
| POST | `/reset-password` | Pending (`reset-password`) | Check the code, save the new hash, set `passwordChangedAt`, mark verified, clear the code, redirect to `/login` with a message | US-11 |
| POST | `/reset-password/resend` | Pending (`reset-password`) | New code, 60 seconds after the last one | US-11 |

### 3.4 Google

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/auth/google` | Guest | Start Google OAuth with a `state` value kept in the session | US-12 |
| GET | `/auth/google/callback` | Public | Check `state`. Find the user by `googleId`, or link by a verified email, or create a verified user with a generated username. Copy the Google photo for a new user or an account without an image. Set the JWT, redirect to `/books`. If the user cancels, redirect to `/login` with a message | US-12, US-13, US-14 |

## 4. Library and books

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/books` | Login | The library: 12 books per page, newest first. Query: `q`, `status`, `genre`, `page` | US-15, US-16, US-17, US-18 |
| GET | `/books/new` | Login | Add book form | US-19 |
| POST | `/books` | Login | Validate. Upload the cover if sent. Create the book with the rules for status, pages, and dates. Redirect to `/books` with a message | US-19, US-24 |
| GET | `/books/:id` | Login | Book details and its notes | US-20 |
| GET | `/books/:id/edit` | Login | Edit form, pre-filled | US-21 |
| PUT | `/books/:id` | Login | Validate, update, apply the status, page, and date rules. A new cover replaces the old one (the old file is deleted). Redirect to `/books/:id` | US-21, US-24 |
| DELETE | `/books/:id` | Login | Delete the cover on Cloudinary, the notes, and the book. Redirect to `/books` (an out of range page becomes the last page) | US-22 |

Privacy is in US-23: every query above includes `owner`.

### 4.1 Query parameters for `/books`

| Param | Values | Rule |
|---|---|---|
| `q` | text, trimmed, up to 100 characters | Matches title or author, not case sensitive. Escaped, never used as a pattern |
| `status` | `want-to-read`, `reading`, `finished` | Any other value is ignored (all) |
| `genre` | a value of the genre list | Any other value is ignored (all) |
| `page` | whole number, 1 or more | Not valid: page 1. Above the last page: the last page |

All can combine: `/books?q=atomic&status=reading&genre=self-help&page=2`. When the user changes `q`, `status`, or `genre`, the page goes back to 1.

### 4.2 Dynamic search and filter

- No new URL. The same `GET /books` is used.
- Browser JavaScript calls `GET /books?...` with the header `X-Requested-With: XMLHttpRequest`.
- With that header the server returns only the results part (cards, count, and pagination bar). Without it, the server returns the full page.
- The browser swaps the results part, keeps the search field and its focus, ignores an older response that arrives late, and updates the URL.
- Without JavaScript, pressing Enter in the search box submits the form as a normal `GET`.
- Same owner filter and same validation in both cases.

## 5. Notes

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| POST | `/books/:id/notes` | Login | Validate. Check that `page` is not above the book's `totalPages`. Create the note. Redirect to `/books/:id` | US-25 |
| GET | `/books/:id/notes/:noteId/edit` | Login | Edit note form, pre-filled | US-26 |
| PUT | `/books/:id/notes/:noteId` | Login | Update `text` and `page` with the same checks. Redirect to `/books/:id` | US-26 |
| DELETE | `/books/:id/notes/:noteId` | Login | Delete the note. Redirect to `/books/:id` | US-27 |

Privacy is in US-28: a note must belong to both the user and the book in the URL.

## 6. Profile

| Method | URL | Access | What it does | Story |
|---|---|---|---|---|
| GET | `/profile` | Login | Profile page: details, library numbers, and the Security section | US-30 |
| GET | `/profile/edit` | Login | Edit form: username and photo | US-31, US-32 |
| PUT | `/profile` | Login | Validate. Update the username. Upload, replace, or remove the photo (the old file is deleted). Redirect to `/profile` | US-31, US-32 |
| GET | `/profile/password` | Login | Change password form. A Google-only account sees Set a password, without the current password field | US-33, US-34 |
| PUT | `/profile/password` | Login | Check the current password (not for a Google-only account). Save the new hash, set `passwordChangedAt`, issue a new JWT, send the notice email, redirect to `/profile` | US-33, US-34 |

There is no `:id` in these URLs. The user comes from the JWT, so nobody can open another profile.

### 6.1 Navbar links (US-29, no route of its own)

| Item | Goes to |
|---|---|
| Logo | `/` (a logged-in user is then sent to `/books`) |
| Visitor: Login, Register | `/login`, `/register` |
| User: My Library | `/books` |
| User: Add Book | `/books/new` |
| User: avatar and username | `/profile` |
| User: Logout | `POST /logout` |

## 7. Request inputs

| Route | Fields |
|---|---|
| POST `/register` | username, email, password, confirmPassword, avatar (file, optional) |
| POST `/verify-email` | code (the six boxes are joined into one field) |
| POST `/login` | username, password, rememberMe |
| POST `/forgot-password` | email |
| POST `/reset-password` | code, password, confirmPassword |
| POST `/books` and PUT `/books/:id` | title, author, genre, description, totalPages, currentPage, status, cover (file, optional) |
| POST `/books/:id/notes` and PUT note | text, page (optional) |
| PUT `/profile` | username, avatar (file, optional), removeAvatar |
| PUT `/profile/password` | currentPassword (not for a Google-only account), password, confirmPassword |

- `password` is always the new password. `confirmPassword` is checked against it and then dropped. It is never saved.
- The server never reads `owner`, `book`, ids, `isVerified`, `passwordChangedAt`, or avatar and cover fields from the form.
- Every field is checked with Joi. An unknown field is rejected.

### 7.1 Uploads

| Field | Rules |
|---|---|
| `cover`, `avatar` | One file, JPG, PNG, or WEBP, up to 2 MB. Type and size are checked on the server |
| Cloudinary folders | `bookshelf/covers`, `bookshelf/avatars` |
| Failed validation | The file that was just uploaded is deleted |
| Replace or remove | The old file is deleted |

## 8. Rate limits

| Route | Limit |
|---|---|
| POST `/register` | 10 per hour per IP |
| POST `/login` | 10 per 15 minutes per IP |
| POST `/verify-email` | 10 per 15 minutes per IP |
| POST `/verify-email/resend` | 5 per 15 minutes per IP, plus 60 seconds per user |
| POST `/forgot-password` | 5 per 15 minutes per IP |
| POST `/reset-password` | 10 per 15 minutes per IP |
| POST `/reset-password/resend` | 5 per 15 minutes per IP, plus 60 seconds per user |
| PUT `/profile/password` | 5 per 15 minutes per user |

Over the limit: status 429, and the form shows "Too many attempts. Try again in 15 minutes."

## 9. Middleware order

| Route type | Order |
|---|---|
| Public and Guest pages | rate limit (if listed), guest check, controller |
| Login routes | `loadUser`, `requireLogin`, controller |
| Routes with a file | rate limit, `requireLogin` (if needed), upload, validate, controller. The controller deletes the uploaded file if validation fails |
| Routes with `:id` | `requireLogin`, validate the id, load the record with `owner`, controller |

## 10. Messages (flash)

Shown once, on the next page. Only the first word is capitalized.

| Action | Type | Message |
|---|---|---|
| Register | info | We sent a code to your email. |
| Code resent | info | We sent you a new code. |
| Email verified | success | Your email is verified. Welcome to BookShelf. |
| Login | success | Welcome back. |
| Logout | success | You are logged out. |
| Not logged in | warning | Log in to continue. |
| Forgot password | info | If an account exists, we sent a code. |
| Password reset | success | Your password was changed. Log in with the new one. |
| Google login canceled | warning | Google login was canceled. |
| Add, edit, delete book | success | Book added. / Book updated. / Book deleted. |
| Add, edit, delete note | success | Note added. / Note updated. / Note deleted. |
| Profile updated | success | Profile updated. |
| Password changed or set | success | Password changed. / Password set. |
| Failed action | error | Something went wrong. Try again. |

## 11. Responses and errors

| Case | Response |
|---|---|
| Unknown URL | 404 page |
| Book, note, or page that is not found or not yours | 404 page |
| Not logged in on a Login route | Redirect to `/` with a message |
| Logged in on a Guest route | Redirect to `/books` |
| Missing pending email | Redirect to the first page of that flow |
| Validation error | Show the form again (status 422) with the errors and the entered values. Passwords are never filled back |
| File error (type or size) | Show the form again with an error under the file field |
| Too many requests | 429, with the message above |
| Google login canceled or failed | Redirect to `/login` with a message |
| Server error | 500 page, no technical details |

## 12. Not routes

- US-5 (remove unverified accounts) is a daily job, not a route.
- US-35 to US-37 (validation, messages, error pages) apply to all routes above.

## 13. Requirements covered

| Area | Requirements | Routes |
|---|---|---|
| Landing page | FR-1 to FR-3 | `/` |
| Register and verify | FR-4 to FR-12 | `/register`, `/verify-email` and resend |
| Login and session | FR-13 to FR-18 | `/login`, `/logout`, Login access level |
| Forgot and reset | FR-19 to FR-23 | `/forgot-password`, `/reset-password` and resend |
| Google | FR-24 to FR-28 | `/auth/google`, `/auth/google/callback` |
| Library | FR-29 to FR-37 | `GET /books` and its query |
| Books | FR-38 to FR-47 | `/books/new`, `/books/:id`, edit, delete |
| Cover | FR-48 to FR-50 | `POST /books`, `PUT /books/:id` |
| Notes | FR-51 to FR-54 | `/books/:id/notes` routes |
| Profile | FR-55 to FR-65 | `/profile`, `/profile/edit`, `/profile/password` |
| Feedback | FR-66 to FR-69 | Rules, messages, and responses above |