# PRD: BookShelf v1

**Author:** Srinivas
**Date:** 15 Oct 2026
**Status:** Draft 2
**Source:** docs/00-client-request.md

## 1. Overview

BookShelf is a web app where a reader keeps a private reading library. The reader tracks books they want to read, are reading, and have finished, with cover images, reading progress, and personal notes. Each reader has a profile with a photo and their reading numbers. The app works on phones, tablets, and laptops.

## 2. Goals

- G1: One place to track all books, reading status, progress, and notes.
- G2: Each reader's library and account are private and secure.
- G3: Any book is quick to find as the library grows.
- G4: The app is comfortable to use on a phone, a tablet, and a laptop or PC.

## 3. Users

| User | Description |
|---|---|
| Visitor | Not logged in. Can see the landing page and the register, login, forgot password, and reset password pages. |
| Reader | A registered and verified user (email or Google). Manages their own library, notes, and profile. |

## 4. Functional requirements

### 4.1 Index

| Area | Requirements |
|---|---|
| Landing page | FR-1 to FR-3 |
| Authentication: register and verify email | FR-4 to FR-12 |
| Authentication: login and session | FR-13 to FR-18 |
| Authentication: forgot and reset password | FR-19 to FR-23 |
| Authentication: Google | FR-24 to FR-28 |
| Library | FR-29 to FR-37 |
| Books | FR-38 to FR-47 |
| Cover image | FR-48 to FR-50 |
| Notes | FR-51 to FR-54 |
| Profile | FR-55 to FR-65 |
| Feedback and validation | FR-66 to FR-69 |

### 4.2 Landing page

- FR-1: A landing page at `/` explains what BookShelf is and lists its main features.
- FR-2: The landing page has clear links to Register and Login.
- FR-3: A logged-in user who opens `/` is sent to their library at `/books`.

### 4.3 Authentication: register and verify email

- FR-4: A visitor can register with username, email, password, and confirm password. Both password fields must match. A profile photo can be added at this step (optional).
- FR-5: A username is unique, 3 to 30 characters, with lowercase letters, numbers, and underscore only. An email is valid and unique.
- FR-6: A password has at least 8 characters, with at least one letter and one number. The same rule applies on register, reset, and change password.
- FR-7: Passwords are stored hashed, never in plain text.
- FR-8: After register, a 6 digit code (OTP) is emailed. The account stays unverified until the correct code is entered, and an unverified user cannot log in.
- FR-9: The code is entered in six separate boxes, one digit per box. Pasting a code fills all six boxes.
- FR-10: A code expires after 10 minutes, is stored hashed, and allows at most 5 wrong attempts. After that the user must request a new code.
- FR-11: A user can request a new code. There must be 60 seconds between requests. A new code makes the old one invalid.
- FR-12: An account that is not verified within 24 hours is deleted, together with its profile image.

### 4.4 Authentication: login and session

- FR-13: A user can log in with username and password.
- FR-14: Wrong credentials show one generic message that does not say which part was wrong.
- FR-15: The login page has a "Remember me" checkbox. Unchecked: the session ends when the browser closes. Checked: the session lasts 30 days.
- FR-16: The session uses a JWT stored in an httpOnly cookie.
- FR-17: A user can log out. Logout clears the cookie and sends the user to the landing page.
- FR-18: Library and profile pages are blocked for visitors. A visitor is sent to the landing page.

### 4.5 Authentication: forgot and reset password

- FR-19: The login page has a "Forgot password" link.
- FR-20: The user enters their email. If an account with a password exists, a 6 digit code is emailed. The same message is shown whether or not the account exists.
- FR-21: The user enters the code, a new password, and a confirm password. The password rule (FR-6) and the code rules (FR-10, FR-11) apply.
- FR-22: A Google-only account gets no code. The email tells the owner to use Google login.
- FR-23: After a reset, the user is sent to login with a success message. An unverified account becomes verified. All sessions of the account end.

### 4.6 Authentication: Google

- FR-24: A user can register or log in with Google (OAuth).
- FR-25: Google users skip the code, because Google has verified the email.
- FR-26: If a Google email matches an existing account and Google reports the email as verified, both are linked to one account. No duplicate account is created.
- FR-27: A new Google user gets a generated username (from the Google name, lowercase, no spaces, plus 4 random digits). The user can change it later.
- FR-28: A new Google user's Google photo is copied and used as the profile image. An existing profile image is never overwritten by Google.

### 4.7 Library

- FR-29: A logged-in user sees all of their own books on one page, the library at `/books`. Each book is a card with cover, title, author, genre, status, and reading progress.
- FR-30: The library is paginated: 12 books per page, newest added first. There is no infinite scroll.
- FR-31: The page number is in the URL. Search and filters stay when the page changes. A page number that is not valid shows page 1. A page number above the last page shows the last page.
- FR-32: A user can search their own books by title or author, not case sensitive.
- FR-33: A user can filter their books by status and by genre. Search, status, and genre work together.
- FR-34: Search and filters update the list on their own, with no Apply button and no full page reload. Search waits about 400 ms after typing stops. Status and genre update immediately.
- FR-35: Any change to search or filters goes back to page 1 and updates the URL.
- FR-36: A Clear button resets search and filters.
- FR-37: An empty library and a search with no match each show a helpful message.

### 4.8 Books

- FR-38: A user can add a book with title, author, genre, description, total pages, current page, and status.
- FR-39: Status is one of: want to read, reading, finished.
- FR-40: Genre is chosen from a fixed list: fiction, non-fiction, fantasy, sci-fi, mystery, thriller, romance, biography, history, self-help, science, technology, business, poetry, other.
- FR-41: Current page cannot be more than total pages. Want to read means current page 0. Finished means current page equals total pages.
- FR-42: Start date and finish date are recorded automatically when the status changes to reading or finished.
- FR-43: Each book shows its reading progress as a percentage of current page over total pages.
- FR-44: A user can open one book to see its details and notes.
- FR-45: A user can edit their own book.
- FR-46: A user can delete their own book after a confirmation. Deleting a book also deletes its notes and its cover image.
- FR-47: A user cannot see, edit, or delete another user's book.

### 4.9 Cover image

- FR-48: A user can add a cover image to a book (optional) when adding or editing, and can replace it.
- FR-49: A cover is JPG, PNG, or WEBP, up to 2 MB. Anything else is rejected with a clear message.
- FR-50: A book without a cover shows a placeholder.

### 4.10 Notes

- FR-51: A user can add many notes to a book. A note has text and an optional page number, which cannot be more than the book's total pages.
- FR-52: Each note shows its created date. Notes show newest first. An edited note shows an "edited" label.
- FR-53: A user can edit their own notes, and delete them after a confirmation.
- FR-54: A user cannot see or change another user's notes.

### 4.11 Profile

- FR-55: The navbar for a logged-in user shows My Library (to the library), Add Book, and the user's avatar and username (to the profile page). There is no sidebar.
- FR-56: The profile page shows the avatar, username, email, member since date, and sign-in method (password or Google).
- FR-57: The profile page shows library information: total books, books per status, pages read, and total notes. Each status card opens the library filtered by that status.
- FR-58: A user can change their username. It follows the username rule (FR-5), and a taken username shows an error.
- FR-59: A user can add a profile image at register (optional), and upload, replace, or remove it at any time (JPG, PNG, or WEBP, up to 2 MB). Without an image the app shows the first letter of the username in a circle.
- FR-60: The profile page has a Security section with a Change password button.
- FR-61: To change a password the user enters the current password, a new password, and a confirm password. The new password follows the password rule (FR-6), matches the confirm field, and differs from the current password.
- FR-62: After a password change, every other session of the account ends and the current session stays.
- FR-63: A Google-only account has no current password. It can set a password without one, and then log in with username and password too.
- FR-64: An email is sent after a password is changed or set, so the owner notices unwanted changes.
- FR-65: A user can only view and edit their own profile.

### 4.12 Feedback and validation

- FR-66: Forms are checked on the client and on the server. An error names the field and the problem, and the entered values are kept.
- FR-67: A validation error under a field disappears as soon as the user starts typing in that field.
- FR-68: A success or error message shows after each action (register, verify, login, logout, add, edit, delete, and password changes). A message can be closed.
- FR-69: An unknown URL, or a book, note, or page the user does not own, shows a 404 page. A server error shows a 500 page without details.

## 5. Non-functional requirements

- NFR-1: The app works on three device classes: mobile (up to 599 px wide, designed at 390 px), tablet (600 to 899 px, designed at 768 px), and laptop or PC (900 px and above, designed at 1440 px). It is designed and built mobile first.
- NFR-2: There is no horizontal scrolling from 320 px wide up. Touch targets are at least 44 px.
- NFR-3: Pages feel fast. Search results appear within about 1 second. The library loads one page of 12 books at a time, and images are served in reduced size by Cloudinary.
- NFR-4: The look follows the design system (`docs/08-design-system.md`): a library card catalog feel, the color tokens, and the fonts Gentium Book Plus and Inter.
- NFR-5: Text contrast is at least 4.5 to 1, and borders, icons, and the focus ring at least 3 to 1 (WCAG 2.1 AA). Every clickable item has a visible focus ring. Every field has a visible label. Status is never shown by color alone. The app can be used with a keyboard.
- NFR-6: Passwords are hashed with bcrypt. Secrets (database URL, JWT secret, session secret, API keys) are only in `.env`. Cookies are httpOnly and sameSite, and secure in production. Every query on books and notes is filtered by owner. Uploaded files are checked for type and size on the server.
- NFR-7: Register, login, code verification, code resend, forgot password, reset password, and change password are rate limited to block brute force and abuse.
- NFR-8: Input is validated in three layers: client JavaScript, server (Joi), and database (Mongoose).
- NFR-9: The app works in the latest two versions of Chrome, Safari, Firefox, and Edge, and in Safari and Chrome on phones and tablets.
- NFR-10: Automated tests (Jest and Supertest) cover the auth, books, notes, and profile routes. Each user story is tested against its acceptance criteria before it is done.
- NFR-11: The app is deployed to a public URL with MongoDB Atlas, and secrets are set as environment variables on the server.

## 6. Out of scope (v1)

- AI chat or recommendations.
- Social features: follow, share, comments.
- Reviews and ratings. Personal notes only.
- A native mobile app.
- Fetching book data from Open Library or Google Books.
- Other login providers (GitHub, Facebook).
- Changing the email address and deleting the account.
- Dark mode.
- Infinite scroll. Pagination is used.
- CSRF tokens (a later hardening step, cookies are sameSite for now).

## 7. Tech stack

- Backend: Node.js, Express
- Database: MongoDB (Mongoose)
- Views: EJS, plain CSS, plain JavaScript
- Validation: Joi
- Auth: bcrypt, JWT in an httpOnly cookie, Passport (Google OAuth 2.0)
- Sessions: `express-session` with `connect-mongo`, only for flash messages, OAuth state, and the pending email
- Email: Nodemailer (codes and notices)
- Uploads: Multer, Cloudinary
- Security: helmet, express-rate-limit
- Jobs: node-cron (cleanup of unverified accounts)
- Tools: Figma (design), Jira (planning), Git and GitHub, Jest and Supertest (tests)

## 8. Assumptions

- One reader has one private library.
- The app is in English only.
- One verified account per email.
- The Cloudinary free plan is enough for images.
- Emails are sent through one provider (a Gmail app password, or the free plan of Brevo or Resend).

## 9. Success criteria

- Every requirement passes the acceptance criteria of its user story.
- The app is deployed at a public URL and is usable on a phone, a tablet, and a laptop.
- The built screens match the Figma designs.
- These flows work end to end: register and verify, log in, add a book with a cover, update progress, write a note, search and filter, edit the profile, change the password, log out.