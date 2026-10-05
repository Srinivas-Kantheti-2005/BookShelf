# User Stories: BookShelf v1

**Source:** docs/01-prd.md
**Date:** 5 Oct 2026

Status key: [ ] not done, [x] done.

---

## Epic 1: Authentication

### US-1: Register
As a visitor, I want to create an account, so that I get my own private library.
**Covers:** FR-1, FR-3
**Acceptance criteria**
- [ ] Form asks username, email, password.
- [ ] Duplicate username or email is rejected with clear error.
- [ ] Password is stored hashed in DB.
- [ ] After register, user is logged in and sent to `/books`.

### US-2: Login
As a user, I want to log in, so that I can reach my library.
**Covers:** FR-2, FR-4
**Acceptance criteria**
- [ ] Correct email and password logs in.
- [ ] Wrong credentials show one generic error (do not say which was wrong).
- [ ] JWT is set in httpOnly cookie.
- [ ] After login, user is sent to `/books`.

### US-3: Logout
As a user, I want to log out, so that nobody else can use my account on this device.
**Covers:** FR-2, FR-27
**Acceptance criteria**
- [ ] Logout clears the cookie.
- [ ] After logout, user is sent to the landing page `/`.
- [ ] After logout, `/books` redirects to the landing page.

### US-4: Protected pages
As a visitor, I should not see library pages, so that data stays private.
**Covers:** FR-5
**Acceptance criteria**
- [ ] Visiting any `/books` URL without login redirects to the landing page `/`.
- [ ] Expired or tampered token is treated as not logged in.

---

## Epic 2: Books

### US-5: Add a book
As a user, I want to add a book, so that it is in my library.
**Covers:** FR-6, FR-7
**Acceptance criteria**
- [ ] Form has title, author, description, status.
- [ ] Status choices: want to read, reading, finished.
- [ ] Title, author, status are required.
- [ ] Saved book appears in my library.

### US-6: See all my books
As a user, I want to see all my books on one page, so that I know my library at a glance.
**Covers:** FR-8
**Acceptance criteria**
- [ ] Each book shows as a card with title, author, cover, status.
- [ ] Status is visually clear (color or label).
- [ ] Empty library shows a helpful empty message.

### US-7: View book details
As a user, I want to open one book, so that I see all its info and notes.
**Covers:** FR-9
**Acceptance criteria**
- [ ] Detail page shows title, author, description, status, cover, notes.
- [ ] Unknown book id shows a not-found page.

### US-8: Edit a book
As a user, I want to edit a book, so that I can fix mistakes or change status.
**Covers:** FR-10
**Acceptance criteria**
- [ ] Edit form is pre-filled with current values.
- [ ] Saving updates the book.
- [ ] Invalid input is rejected with clear error.

### US-9: Delete a book
As a user, I want to delete a book, so that I can remove what I no longer need.
**Covers:** FR-11, FR-19
**Acceptance criteria**
- [ ] Confirmation appears before delete.
- [ ] Cancel keeps the book.
- [ ] Confirm deletes the book and all its notes.

### US-10: Books are private
As a user, I should only touch my own books, so that my library stays private.
**Covers:** FR-12
**Acceptance criteria**
- [ ] Opening another user's book URL shows not-found or forbidden.
- [ ] Edit and delete on another user's book are blocked.
- [ ] Library list shows only my books.

---

## Epic 3: Cover Image

### US-11: Upload cover
As a user, I want to add a cover image, so that my library looks nice.
**Covers:** FR-13, FR-14
**Acceptance criteria**
- [ ] Cover upload is optional on add and edit.
- [ ] Only image files accepted (jpg, png, webp).
- [ ] File over size limit is rejected with clear error.
- [ ] Book without cover shows a default placeholder.

---

## Epic 4: Notes

### US-12: Add a note
As a user, I want to add notes to a book, so that I keep my thoughts and quotes.
**Covers:** FR-15, FR-16
**Acceptance criteria**
- [ ] Note has text and optional page number.
- [ ] A book can have many notes.
- [ ] Each note shows its created date.
- [ ] Empty text is rejected.

### US-13: Edit a note
As a user, I want to edit a note, so that I can fix or improve it.
**Covers:** FR-17
**Acceptance criteria**
- [ ] Edit form is pre-filled.
- [ ] Saving updates the note.

### US-14: Delete a note
As a user, I want to delete a note, so that I can remove what I don't need.
**Covers:** FR-17
**Acceptance criteria**
- [ ] Confirmation appears before delete.
- [ ] Only that note is removed.

### US-15: Notes are private
As a user, I should only see my own notes, so that my thoughts stay private.
**Covers:** FR-18
**Acceptance criteria**
- [ ] Another user cannot view, edit, or delete my notes by URL.

---

## Epic 5: Search and Filter

### US-16: Search books
As a user, I want to search by title or author or genre, so that I find a book fast.
**Covers:** FR-20
**Acceptance criteria**
- [ ] Search matches title or author, not case sensitive.
- [ ] Only my books are searched.
- [ ] No match shows a "nothing found" message.

### US-17: Filter by status
As a user, I want to filter by status, so that I see only what I am reading or want to read.
**Covers:** FR-21
**Acceptance criteria**
- [ ] Filter choices: all, want to read, reading, finished.
- [ ] Filter works together with search.

---

## Epic 6: Feedback and Validation

### US-18: Form validation
As a user, I want clear errors on bad input, so that I know how to fix it.
**Covers:** FR-22
**Acceptance criteria**
- [ ] Validation runs on client (JS) and server (Joi).
- [ ] Error message names the field and the problem.
- [ ] Entered values are kept after an error.

### US-19: Action messages
As a user, I want a message after each action, so that I know it worked.
**Covers:** FR-23
**Acceptance criteria**
- [ ] Success message after add, edit, delete, login, logout.
- [ ] Error message when action fails.
- [ ] Message disappears or can be closed.

---

## Epic 7: Landing Page

### US-20: Landing page
As a visitor, I want to see what BookShelf is, so that I can decide to join.
**Covers:** FR-24, FR-25, FR-26
**Acceptance criteria**
- [ ] `/` explains what BookShelf is and lists its main features.
- [ ] Page has clear Register and Login links.
- [ ] Logged-in user opening `/` is sent to `/books`.
- [ ] Page looks fine on phone and desktop.