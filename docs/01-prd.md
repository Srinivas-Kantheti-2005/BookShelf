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
- FR-1: Visitor can register with username, email, password.
- FR-2: User can log in and log out.
- FR-3: Passwords are stored hashed, never plain.
- FR-4: Session uses JWT stored in httpOnly cookie.
- FR-5: Library pages are blocked for visitors (redirect to landing page).

### Books
- FR-6: User can add a book: title, author, description, status.
- FR-7: Status is one of: want to read, reading, finished.
- FR-8: User can see all own books in one page, status visible at a glance.
- FR-9: User can view one book's details.
- FR-10: User can edit own book.
- FR-11: User can delete own book, with a confirmation first.
- FR-12: User cannot see, edit, or delete another user's book.

### Cover image
- FR-13: User can upload a cover image for a book.
- FR-14: Only image files allowed, with a size limit.

### Notes
- FR-15: User can add multiple notes to a book. Each note has text and an optional page number.
- FR-16: Each note shows the date it was created.
- FR-17: User can edit and delete own notes.
- FR-18: User cannot see or change notes of another user.
- FR-19: Deleting a book also deletes all its notes.

### Search and filter
- FR-18: User can search own books by title or author or genre.
- FR-19: User can filter books by status.

### Feedback
- FR-20: Form input is validated, clear error shown on bad input.
- FR-21: Success and error messages shown after actions (flash).

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

## 6. Out of Scope (v1)
- AI chat or recommendations (later version).
- Social features: follow, share, comments.
- Mobile app.
- Fetching book data from Open Library or Google Books.

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