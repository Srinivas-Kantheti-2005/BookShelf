# User Stories: BookShelf v1

**Source:** docs/01-prd.md
**Date:** 5 Oct 2026

Status key: [ ] not done, [x] done.

---

## Epic 1: Authentication

### US-1: Register
As a visitor, I want to create an account, so that I get my own private library.
**Covers:** FR-1, FR-3, FR-29, FR-50
**Acceptance criteria**
- [ ] Form asks username, email, password, confirm password.
- [ ] Password must be at least 8 characters with a letter and a number.
- [ ] Confirm password must match password, otherwise a clear error shows.
- [ ] Duplicate username or email is rejected with clear error.
- [ ] Password is stored hashed in DB. Confirm password is never stored.
- [ ] After submit, a 6 digit code is emailed and user is sent to the verify page.
- [ ] Account is unverified until the code is entered.

### US-2: Login
As a user, I want to log in, so that I can reach my library.
**Covers:** FR-2, FR-4, FR-28, FR-30, FR-42
**Acceptance criteria**
- [ ] Login form has email, password, a Remember me checkbox, and a Forgot password link.
- [ ] Correct email and password logs in.
- [ ] Wrong credentials show one generic error (do not say which was wrong).
- [ ] Unverified user cannot log in and is sent to the verify page.
- [ ] JWT is set in httpOnly cookie.
- [ ] Remember me unchecked: session ends when the browser closes. Checked: session lasts 30 days.
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

### US-21: Verify email with OTP
As a new user, I want to confirm my email with a code, so that my account is real.
**Covers:** FR-29, FR-30, FR-31, FR-32
**Acceptance criteria**
- [ ] Verify page asks for the 6 digit code.
- [ ] Correct code marks account verified, logs the user in, and sends to `/books`.
- [ ] Wrong code shows a clear error.
- [ ] Code expires after 10 minutes.
- [ ] After 5 wrong attempts, user must request a new code.
- [ ] Code is stored hashed in DB.

### US-22: Resend OTP
As a new user, I want a new code, so that I can verify if the first one is lost or expired.
**Covers:** FR-33
**Acceptance criteria**
- [ ] Verify page has a Resend code button.
- [ ] User must wait 60 seconds between requests.
- [ ] A new code makes the old code invalid.

### US-23: Remember me
As a user, I want to stay logged in on my device, so that I don't log in every time.
**Covers:** FR-28
**Acceptance criteria**
- [ ] Login form has a Remember me checkbox.
- [ ] Unchecked: session ends when the browser closes.
- [ ] Checked: session lasts 30 days.

### US-24: Login with Google
As a user, I want to continue with Google, so that I skip typing a password.
**Covers:** FR-34, FR-35
**Acceptance criteria**
- [ ] Login and register pages have a Continue with Google button.
- [ ] First time Google user gets an account, already verified, with no OTP.
- [ ] Returning Google user is logged in.
- [ ] After login, user is sent to `/books`.
- [ ] If user cancels on Google, they return to login with a message.

### US-25: Account linking
As a user, I want one account for my email, so that I don't get duplicates.
**Covers:** FR-36
**Acceptance criteria**
- [ ] Google email matching an existing account links to that account.
- [ ] No duplicate account is created.
- [ ] User can log in with either method afterwards.

### US-26: Forgot password
As a user, I want to reset my password, so that I can get back into my account if I forget it.
**Covers:** FR-42, FR-43, FR-44, FR-45, FR-46, FR-47, FR-50
**Acceptance criteria**
- [ ] Login page has a Forgot password link.
- [ ] User enters email and always sees the same message: "If an account exists, we sent a code."
- [ ] A 6 digit code is emailed only to accounts that have a password.
- [ ] Google-only account gets an email telling them to use Google login.
- [ ] Reset page asks for code, new password, and confirm password.
- [ ] New password follows the same rule as register (FR-50) and must match confirm password.
- [ ] Correct code with a valid matching password saves the new hashed password.
- [ ] Code expires after 10 minutes and allows max 5 wrong attempts.
- [ ] User can request a new code after 60 seconds.
- [ ] After reset, user is sent to login with a success message.

---

## Epic 2: Books

### US-5: Add a book
As a user, I want to add a book, so that it is in my library.
**Covers:** FR-6, FR-7, FR-37, FR-38, FR-40
**Acceptance criteria**
- [ ] Form has title, author, genre, description, total pages, current page, status.
- [ ] Genre is chosen from a fixed list.
- [ ] Status choices: want to read, reading, finished.
- [ ] Title, author, genre, total pages, status are required.
- [ ] Current page cannot be more than total pages.
- [ ] Want to read sets current page to 0. Finished sets current page equal to total pages.
- [ ] Start date is saved when status is reading. Finish date is saved when status is finished.
- [ ] Saved book appears in my library.

### US-6: See all my books
As a user, I want to see all my books on one page, so that I know my library at a glance.
**Covers:** FR-8, FR-39, FR-48, FR-49
**Acceptance criteria**
- [ ] Each book shows as a card with title, author, genre, cover, status.
- [ ] Card shows a progress bar and percentage (current page out of total pages).
- [ ] Status is visually clear (color or label).
- [ ] Library shows 12 books per page, newest added first.
- [ ] Pagination bar shows Previous, page numbers, and Next. It is hidden when there is only one page.
- [ ] Previous is disabled on the first page. Next is disabled on the last page.
- [ ] Page number is in the URL, and search and filters are kept when changing pages.
- [ ] Applying a search or filter goes back to page 1.
- [ ] Invalid page number shows page 1. Too-high page number shows the last page.
- [ ] Empty library shows a helpful empty message.

### US-7: View book details
As a user, I want to open one book, so that I see all its info and notes.
**Covers:** FR-9, FR-39
**Acceptance criteria**
- [ ] Detail page shows title, author, genre, description, status, cover, pages, progress, notes.
- [ ] Start date and finish date show when they exist.
- [ ] Unknown book id shows a not-found page.

### US-8: Edit a book
As a user, I want to edit a book, so that I can fix mistakes, update my current page, or change status.
**Covers:** FR-10, FR-38, FR-40
**Acceptance criteria**
- [ ] Edit form is pre-filled with current values.
- [ ] Saving updates the book.
- [ ] Current page cannot be more than total pages.
- [ ] Changing status to reading, finished, or want to read follows the page and date rules from US-5.
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
- [ ] Page number cannot be more than the book's total pages.
- [ ] A book can have many notes.
- [ ] Each note shows its created date. Newest note shows first.
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
As a user, I want to search by title or author, so that I find a book fast.
**Covers:** FR-20, FR-51, FR-52
**Acceptance criteria**
- [ ] Search matches title or author, not case sensitive.
- [ ] Only my books are searched.
- [ ] Results update while I type, after a short pause (about 400 ms), with no Apply button and no full page reload.
- [ ] The search field keeps my text and focus while results update.
- [ ] URL updates with the search text, so refresh, Back, and bookmarks work.
- [ ] Changing the search goes back to page 1.
- [ ] No match shows a "nothing found" message.

### US-17: Filter by status and genre
As a user, I want to filter by status and genre, so that I see only what I want.
**Covers:** FR-21, FR-41, FR-51, FR-52
**Acceptance criteria**
- [ ] Status filter choices: all, want to read, reading, finished.
- [ ] Genre filter has all genres from the fixed list, plus all.
- [ ] The list updates as soon as I pick a value, with no Apply button and no full page reload.
- [ ] Status filter, genre filter, and search work together.
- [ ] URL updates with the chosen filters.
- [ ] Changing a filter goes back to page 1.
- [ ] Clear button resets search and filters and shows the full library.

---

## Epic 6: Feedback and Validation

### US-18: Form validation
As a user, I want clear errors on bad input, so that I know how to fix it.
**Covers:** FR-22, FR-53
**Acceptance criteria**
- [ ] Validation runs on client (JS) and server (Joi).
- [ ] Error message names the field and the problem.
- [ ] Entered values are kept after an error.
- [ ] An error under a field disappears as soon as I start typing in that field.
- [ ] If the value is still invalid on submit, the error shows again.

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