# Routes: BookShelf v1

**Source:** docs/01-prd.md, docs/02-user-stories.md, docs/03-database-design.md
**Date:** 6 Oct 2026

## 1. Rules
- Server-rendered EJS pages. Forms send `GET` or `POST` only.
- `PUT` and `DELETE` use `method-override` (`?_method=PUT`, `?_method=DELETE`).
- Logout is `POST`, not `GET`, so a link or image cannot log a user out.
- Auth column: **Public** = anyone. **Login** = valid JWT cookie and verified account, else redirect to `/`.
- Every `:id` route also checks the record belongs to the logged-in user. Not yours means 404.
- Cover upload forms use `multipart/form-data`.
- Pending verify and reset emails are kept in the server session (15 minute limit), not in the URL.

## 2. Public and auth routes

| Method | URL | Auth | What it does | Story |
|---|---|---|---|---|
| GET | `/` | Public | Landing page. Logged-in user is sent to `/books` | US-20 |
| GET | `/register` | Public | Register form | US-1 |
| POST | `/register` | Public | Validate, create unverified user, send OTP, redirect to `/verify-email` | US-1 |
| GET | `/verify-email` | Public | OTP form. No pending email cookie means redirect to `/register` | US-21 |
| POST | `/verify-email` | Public | Check code. Success: verify, set JWT, redirect to `/books` | US-21 |
| POST | `/verify-email/resend` | Public | New OTP, 60 second wait | US-22 |
| GET | `/login` | Public | Login form with Remember me | US-2, US-23 |
| POST | `/login` | Public | Check credentials, set JWT cookie, redirect to `/books`. Unverified user goes to `/verify-email` | US-2, US-23 |
| GET | `/forgot-password` | Public | Forgot password form | US-26 |
| POST | `/forgot-password` | Public | Send reset code if the account exists, always show the same message, redirect to `/reset-password` | US-26 |
| GET | `/reset-password` | Public | Reset form. No pending email cookie means redirect to `/forgot-password` | US-26 |
| POST | `/reset-password` | Public | Check code, save new password, redirect to `/login` with success message | US-26 |
| POST | `/reset-password/resend` | Public | New reset code, 60 second wait | US-26 |
| POST | `/logout` | Login | Clear cookie, redirect to `/` | US-3 |
| GET | `/auth/google` | Public | Start Google OAuth | US-24 |
| GET | `/auth/google/callback` | Public | Find, link, or create user, set JWT, redirect to `/books`. Cancel returns to `/login` with message | US-24, US-25 |

## 3. Book routes

| Method | URL | Auth | What it does | Story |
|---|---|---|---|---|
| GET | `/books` | Login | My library, 12 per page, newest first. Query: `q`, `status`, `genre`, `page` | US-6, US-16, US-17 |
| GET | `/books/new` | Login | Add book form | US-5 |
| POST | `/books` | Login | Validate, upload cover, create book, redirect to `/books` | US-5, US-11 |
| GET | `/books/:id` | Login | Book details with notes | US-7 |
| GET | `/books/:id/edit` | Login | Edit form, pre-filled | US-8 |
| PUT | `/books/:id` | Login | Validate, update book, replace cover if new one sent, redirect to `/books/:id` | US-8, US-11 |
| DELETE | `/books/:id` | Login | Delete cover on Cloudinary, delete its notes, delete book, redirect to `/books` | US-9 |

## 4. Note routes

| Method | URL | Auth | What it does | Story |
|---|---|---|---|---|
| POST | `/books/:id/notes` | Login | Validate, create note, redirect to `/books/:id` | US-12 |
| GET | `/books/:id/notes/:noteId/edit` | Login | Edit note form, pre-filled | US-13 |
| PUT | `/books/:id/notes/:noteId` | Login | Update text and page, redirect to `/books/:id` | US-13 |
| DELETE | `/books/:id/notes/:noteId` | Login | Delete note, redirect to `/books/:id` | US-14 |

## 5. Query params for `/books`

| Param | Values | Example |
|---|---|---|
| `q` | text, matches title or author | `/books?q=atomic` |
| `status` | `want-to-read`, `reading`, `finished` | `/books?status=reading` |
| `genre` | any value from the genre list | `/books?genre=sci-fi` |
| `page` | whole number, 1 or more | `/books?page=2` |

All can combine: `/books?q=atomic&status=reading&genre=self-help&page=2`. Changing `q`, `status`, or `genre` resets `page` to 1. Invalid `page` shows page 1. Too-high `page` shows the last page.

### 5.1 Dynamic search and filter

- No new URL. The same `GET /books` is used.
- Browser JavaScript calls `GET /books?...` with the header `X-Requested-With: XMLHttpRequest`.
- With that header the server returns only the results part (cards, count, pagination bar) instead of the full page.
- Without the header the server returns the full page, so Enter, refresh, Back, and bookmarks work.
- Same owner filter, same validation of `q`, `status`, `genre`, `page` in both cases.

## 6. Request inputs

| Route | Body fields |
|---|---|
| POST `/register` | username, email, password, confirmPassword |
| POST `/verify-email` | code |
| POST `/login` | email, password, rememberMe |
| POST `/forgot-password` | email |
| POST `/reset-password` | code, password, confirmPassword |
| POST, PUT `/books` | title, author, genre, description, totalPages, currentPage, status, cover (file) |
| POST, PUT note | text, page |

`owner`, `book`, and ids are never read from the body. Server sets them.

`confirmPassword` is checked against `password` and then dropped. It is never saved.

## 7. Rate limits
- POST `/login`: 10 per 15 minutes per IP.
- POST `/verify-email`: 10 per 15 minutes per IP.
- POST `/verify-email/resend`: 5 per 15 minutes per IP, plus the 60 second wait per user.
- POST `/forgot-password`: 5 per 15 minutes per IP.
- POST `/reset-password`: 10 per 15 minutes per IP.
- POST `/reset-password/resend`: 5 per 15 minutes per IP, plus the 60 second wait per user.

## 8. Flash messages
Success or error shown after each action (US-19): register, verify, login, logout, add, edit, delete for books and notes.
Flash messages are stored in the session and shown once, on the next page.

## 9. Error pages

| Case | Response |
|---|---|
| Unknown URL | 404 page |
| Book or note not found, or not yours | 404 page |
| Not logged in on a Login route | redirect to `/` |
| Validation error | re-render the form with errors and entered values |
| Server error | 500 page, no stack trace shown |