# PRD: BookShelf v1

**Author:** Srinivas
**Date:** 5 Oct 2026
**Status:** Draft
**Source:** docs/00-client-request.md

## 1. Overview
BookShelf is a web app where a user keeps a private reading library.
User tracks books they want to read, are reading, and have finished,
with cover images and personal notes.

## 2. Goals
- G1: One place to track all books and reading status.
- G2: Each user's library is private and secure.
- G3: Fast to find any book as library grows.

## 3. Users
- Reader: registered user who manages own library.
- Visitor: not logged in. Can see the landing page, login page, and register page only.

## 4. Functional Requirements

### Authentication
- FR-1: Visitor can register with username, email, password, and confirm password. Both password fields must match.
- FR-2: User can log in and log out.
- FR-3: Passwords are stored hashed, never plain.
- FR-4: Session uses JWT stored in httpOnly cookie.
- FR-5: Library pages are blocked for visitors (redirect to landing page).
- FR-42: Login page has a "Forgot password" link.
- FR-43: User enters their email on the forgot password page. If an account with a password exists, a 6 digit code is emailed. The same message is shown whether or not the account exists.
- FR-44: User enters the code, a new password, and a confirm password to reset.
- FR-45: Reset code follows the OTP rules: expires in 10 minutes, stored hashed, max 5 wrong attempts, new code allowed after 60 seconds.
- FR-46: Google-only accounts get no code. The email tells them to use Google login.
- FR-47: After a reset, user is sent to login with a success message. An unverified account becomes verified.
- FR-50: Password must be at least 8 characters with at least one letter and one number. Same rule on register and reset.

### Books
- FR-6: User can add a book: title, author, genre, description, total pages, current page, status.
- FR-7: Status is one of: want to read, reading, finished.
- FR-8: User can see all own books in one page, with status and reading progress visible at a glance.
- FR-9: User can view one book's details.
- FR-10: User can edit own book.
- FR-11: User can delete own book, with a confirmation first.
- FR-12: User cannot see, edit, or delete another user's book.
- FR-37: Genre is chosen from a fixed list (fiction, non-fiction, fantasy, sci-fi, mystery, thriller, romance, biography, history, self-help, science, technology, business, poetry, other).
- FR-38: Current page cannot be more than total pages. Want to read means current page 0. Finished means current page equals total pages.
- FR-39: Each book shows reading progress as a percentage, calculated from current page and total pages.
- FR-40: Start date and finish date are recorded automatically when status changes to reading or finished.
- FR-48: Library list is paginated: 12 books per page, newest added first.
- FR-49: Page number is in the URL. Search and filters stay when changing pages. Invalid page number shows page 1, too-high page number shows the last page.

### Cover image
- FR-13: User can upload a cover image for a book.
- FR-14: Only image files allowed, with a size limit.

### Notes
- FR-15: User can add multiple notes to a book. Each note has text and an optional page number, which cannot be more than the book's total pages.
- FR-16: Each note shows the date it was created.
- FR-17: User can edit and delete own notes.
- FR-18: User cannot see or change notes of another user.
- FR-19: Deleting a book also deletes all its notes.

### Search and filter
- FR-18: User can search own books by title or author or genre.
- FR-19: User can filter books by status.
- FR-41: User can filter books by genre.
- FR-51: Search and filters update the book list on their own, with no Apply button and no full page reload. Search waits about 500 ms after typing stops. Status and genre update immediately.
- FR-52: Any change to search or filters goes back to page 1 and updates the URL.

### Feedback
- FR-20: Form input is validated, clear error shown on bad input.
- FR-21: Success and error messages shown after actions (flash).
- FR-53: A validation error under a field disappears as soon as the user starts typing in that field.

### Landing page
- FR-24: Landing page at `/` explains what BookShelf is and its main features.
- FR-25: Landing page has clear links to Register and Login.
- FR-26: A logged-in user who opens `/` is sent to their library at `/books`.
- FR-27: After logout, user is sent to the landing page.

## 5. Non-Functional Requirements
- NFR-1: Works on desktop and phone browser (responsive).
- NFR-2: Page load feels fast, search responds quickly.
- NFR-3: Secrets (DB url, JWT secret, API keys) only in `.env`.
- NFR-4: Clean card-catalog library look.
- NFR-5: Login, OTP verify, OTP resend, forgot password, and reset password are rate limited to block brute force.

## 6. Out of Scope (v1)
- AI chat or recommendations (later version).
- Social features: follow, share, comments.
- Mobile app.
- Fetching book data from Open Library or Google Books.
- Other OAuth providers (GitHub, Facebook) (later version).
- Infinite scroll (pagination is used instead).

## 7. Tech Stack
- Backend: Node.js, Express
- Database: MongoDB (Mongoose)
- Views: EJS, plain CSS, plain JavaScript
- Validation: Joi
- Auth: bcrypt + JWT
- Uploads: Multer + Cloudinary

## 8. Assumptions
- One user = one private library.
- Single language: English.

## 9. Success Criteria
- All FR items pass their acceptance criteria.
- App deployed and usable from a public URL.