# User Stories: BookShelf v1

**Source:** docs/01-prd.md
**Date:** 15 Oct 2026

Status key: [ ] not done, [x] done.

## Definition of done (applies to every story)

A story is done only when all of these are true, plus its own criteria:

- [ ] It works on mobile (390 px), tablet (768 px), and laptop or PC (1440 px), with no horizontal scrolling from 320 px up (NFR-1, NFR-2).
- [ ] It matches the Figma frames for all three devices.
- [ ] It works with a keyboard only, with a visible focus ring, labeled fields, and readable contrast (NFR-5).
- [ ] Input is validated on the client and on the server (NFR-8).
- [ ] Every query on books and notes is filtered by owner (NFR-6).
- [ ] Its automated tests pass (NFR-10).
- [ ] It is merged into `dev`, and all its subtasks (Design, Backend, Frontend, Test) are done.

## Epic list

| # | Epic | Stories |
|---|---|---|
| 1 | Landing page | US-1 |
| 2 | Register and verify email | US-2 to US-5 |
| 3 | Login and session | US-6 to US-9 |
| 4 | Forgot and reset password | US-10, US-11 |
| 5 | Google login | US-12 to US-14 |
| 6 | Library | US-15 to US-18 |
| 7 | Books | US-19 to US-23 |
| 8 | Cover image | US-24 |
| 9 | Notes | US-25 to US-28 |
| 10 | Profile | US-29 to US-34 |
| 11 | Feedback and validation | US-35 to US-37 |

---

## Epic 1: Landing page

### US-1: Landing page
As a visitor, I want to see what BookShelf is, so that I can decide to join.
**Covers:** FR-1, FR-2, FR-3
**Acceptance criteria**
- [ ] `/` explains what BookShelf is and lists its main features.
- [ ] The page has clear Register and Login links.
- [ ] A logged-in user opening `/` is sent to `/books`.

---

## Epic 2: Register and verify email

### US-2: Register
As a visitor, I want to create an account, so that I get my own private library.
**Covers:** FR-4, FR-5, FR-6, FR-7
**Acceptance criteria**
- [ ] The form asks username, email, password, and confirm password, and has an optional profile photo.
- [ ] Username is 3 to 30 characters: lowercase letters, numbers, and underscore. A taken username shows a clear error.
- [ ] Email must be valid. An email that belongs to a verified account shows a clear error.
- [ ] Registering again with an email that is not yet verified replaces the earlier attempt and sends a new code.
- [ ] Password is at least 8 characters with a letter and a number.
- [ ] Confirm password must match password, otherwise a clear error shows.
- [ ] The photo accepts JPG, PNG, or WEBP up to 2 MB, shows a preview, and shows a clear error otherwise.
- [ ] Without a photo, a circle with the first letter of the username shows, and it updates as I type.
- [ ] A photo uploaded with a form that fails validation is not kept.
- [ ] The password is stored hashed. Confirm password is never stored.
- [ ] After submit, a 6 digit code is emailed and I am sent to the verify page.
- [ ] The account is unverified until the code is entered.
- [ ] Register is rate limited.

### US-3: Verify email with a code
As a new user, I want to confirm my email with a code, so that my account is real.
**Covers:** FR-8, FR-9, FR-10
**Acceptance criteria**
- [ ] The verify page shows six boxes, one digit per box.
- [ ] Typing a digit moves to the next box. Backspace goes back. Pasting a code fills all six.
- [ ] The correct code marks the account verified, logs me in, and sends me to `/books`.
- [ ] A wrong code shows a clear error.
- [ ] The code expires after 10 minutes and is stored hashed.
- [ ] After 5 wrong attempts, I must request a new code.
- [ ] Verifying is rate limited.
- [ ] Opening the verify page without a pending email sends me to register.

### US-4: Resend the code
As a new user, I want a new code, so that I can verify if the first one is lost or expired.
**Covers:** FR-11
**Acceptance criteria**
- [ ] The verify page has a Resend code button.
- [ ] I must wait 60 seconds between requests, and a countdown shows.
- [ ] A new code makes the old code invalid.
- [ ] Resending is rate limited.

### US-5: Remove unverified accounts
As the site owner, I want unverified accounts deleted, so that abandoned signups do not pile up.
**Covers:** FR-12
**Acceptance criteria**
- [ ] A daily job deletes accounts that are not verified within 24 hours.
- [ ] The profile photo of a deleted account is also deleted from Cloudinary.
- [ ] Verified accounts are never deleted.
- [ ] Tested with a fake old unverified account.

---

## Epic 3: Login and session

### US-6: Login
As a user, I want to log in, so that I can reach my library.
**Covers:** FR-13, FR-14, FR-16
**Acceptance criteria**
- [ ] The login form has username, password, a Remember me checkbox, a Forgot password link, a Continue with Google button, and a Register link.
- [ ] A correct username and password logs me in.
- [ ] Wrong credentials show one generic message that does not say which part was wrong.
- [ ] An unverified user cannot log in and is sent to the verify page, where a new code can be requested.
- [ ] The JWT is set in an httpOnly cookie.
- [ ] After login, I am sent to `/books`.
- [ ] Login is rate limited.

### US-7: Remember me
As a user, I want to stay logged in on my device, so that I do not log in every time.
**Covers:** FR-15
**Acceptance criteria**
- [ ] Remember me unchecked: the session ends when the browser closes.
- [ ] Remember me checked: the session lasts 30 days.

### US-8: Logout
As a user, I want to log out, so that nobody else can use my account on this device.
**Covers:** FR-17
**Acceptance criteria**
- [ ] Logout is a button in the navbar and in the mobile menu.
- [ ] Logout clears the cookie and sends me to the landing page with a message.
- [ ] After logout, `/books` and `/profile` redirect to the landing page.

### US-9: Protected pages
As a visitor, I should not see library or profile pages, so that data stays private.
**Covers:** FR-18
**Acceptance criteria**
- [ ] Opening any `/books` or `/profile` URL without login redirects to the landing page `/`.
- [ ] An expired or tampered token is treated as not logged in.
- [ ] A token issued before my last password change is rejected.

---

## Epic 4: Forgot and reset password

### US-10: Forgot password
As a user, I want to ask for a reset code, so that I can get back into my account.
**Covers:** FR-19, FR-20, FR-22
**Acceptance criteria**
- [ ] The Forgot password link on the login page opens the forgot password page.
- [ ] I enter my email and always see the same message: "If an account exists, we sent a code."
- [ ] A 6 digit code is emailed only to accounts that have a password.
- [ ] A Google-only account gets an email telling the owner to use Google login.
- [ ] Requests are rate limited.

### US-11: Reset password
As a user, I want to set a new password with my code, so that I can log in again.
**Covers:** FR-21, FR-23
**Acceptance criteria**
- [ ] The reset page asks for the code, a new password, and a confirm password.
- [ ] The new password follows the password rule and matches the confirm field.
- [ ] The code expires after 10 minutes, allows 5 wrong attempts, and can be resent after 60 seconds.
- [ ] On success, the new password is saved hashed and an unverified account becomes verified.
- [ ] On success, all sessions of the account end, and I am sent to login with a success message.
- [ ] Opening the reset page without a pending email sends me to the forgot password page.

---

## Epic 5: Google login

### US-12: Log in or register with Google
As a user, I want to continue with Google, so that I skip typing a password.
**Covers:** FR-24, FR-25, FR-27
**Acceptance criteria**
- [ ] The login and register pages have a Continue with Google button.
- [ ] A first-time Google user gets an account that is already verified, with no code.
- [ ] A new Google user gets a generated username: from the Google name, lowercase, no spaces, plus 4 random digits.
- [ ] A returning Google user is logged in and sent to `/books`.
- [ ] If I cancel on Google, I return to login with a message.

### US-13: Link Google to an existing account
As a user, I want one account for my email, so that I do not get duplicates.
**Covers:** FR-26
**Acceptance criteria**
- [ ] A Google email that matches an existing account is linked to it, only when Google reports the email as verified.
- [ ] No duplicate account is created.
- [ ] I can log in either way afterwards, if the account has a password.

### US-14: Google profile photo
As a Google user, I want my Google photo as my profile image, so that I do not have to upload one.
**Covers:** FR-28
**Acceptance criteria**
- [ ] A new Google user gets the Google photo as the profile image, copied to Cloudinary.
- [ ] A linked account without a profile image gets the Google photo. An existing image is kept.
- [ ] If the photo cannot be fetched, the account is still created, with the first letter circle.

---

## Epic 6: Library

### US-15: See my library
As a user, I want to see all my books on one page, so that I know my library at a glance.
**Covers:** FR-29, FR-37, FR-43
**Acceptance criteria**
- [ ] `/books` shows only my books, each as a card with cover, title, author, genre, status, and a progress bar with a percentage.
- [ ] Status shows as a pill with text, never color alone.
- [ ] Cards show in 1 column on mobile, 2 on tablet, and 3 on laptop and PC.
- [ ] An empty library shows a helpful message and an Add a book button.
- [ ] A search with no match shows a "Nothing found" message and a Clear link.

### US-16: Paginate the library
As a user, I want my books in pages, so that a big library stays fast and easy to scan.
**Covers:** FR-30, FR-31
**Acceptance criteria**
- [ ] The library shows 12 books per page, newest added first.
- [ ] The pagination bar shows Previous, page numbers, and Next. It is hidden when there is one page.
- [ ] Previous is disabled on the first page. Next is disabled on the last page.
- [ ] On mobile, the bar shows Previous, "Page 2 of 5", and Next.
- [ ] The page number is in the URL, and search and filters stay when the page changes.
- [ ] An invalid page number shows page 1. A page number above the last shows the last page.
- [ ] Changing the page scrolls to the top of the list.

### US-17: Search books
As a user, I want to search by title or author, so that I find a book fast.
**Covers:** FR-32, FR-34, FR-35
**Acceptance criteria**
- [ ] Search matches title or author, not case sensitive, and only my books.
- [ ] Results update while I type, after a short pause of about 400 ms, with no Apply button and no full page reload.
- [ ] The search field keeps my text and focus while results update.
- [ ] Older results never replace newer ones.
- [ ] The URL updates with the search text, so refresh, Back, and bookmarks work.
- [ ] Changing the search goes back to page 1.

### US-18: Filter by status and genre
As a user, I want to filter by status and genre, so that I see only what I want.
**Covers:** FR-33, FR-34, FR-35, FR-36
**Acceptance criteria**
- [ ] The status filter has: all, want to read, reading, finished.
- [ ] The genre filter has all genres from the fixed list, plus all.
- [ ] The list updates as soon as I pick a value, with no Apply button and no full page reload.
- [ ] Status, genre, and search work together.
- [ ] The URL updates with the chosen filters, and changing a filter goes back to page 1.
- [ ] A Clear button resets search and filters and shows the full library.

---

## Epic 7: Books

### US-19: Add a book
As a user, I want to add a book, so that it is in my library.
**Covers:** FR-38, FR-39, FR-40, FR-41, FR-42
**Acceptance criteria**
- [ ] The form has title, author, genre, description, total pages, current page, and status.
- [ ] Title, author, genre, total pages, and status are required. Description is optional.
- [ ] Limits: title 150, author 100, description 2000 characters.
- [ ] Genre is chosen from the fixed list. Status is want to read, reading, or finished.
- [ ] Current page cannot be more than total pages.
- [ ] Want to read sets current page to 0. Finished sets current page equal to total pages.
- [ ] The start date is saved when the status is reading. The finish date is saved when the status is finished.
- [ ] The saved book appears in my library.

### US-20: View book details
As a user, I want to open one book, so that I see all its information and notes.
**Covers:** FR-43, FR-44
**Acceptance criteria**
- [ ] The page shows cover, title, author, genre, status, description, total pages, current page, and progress percentage.
- [ ] Start date and finish date show when they exist.
- [ ] The page shows my notes for this book, and has Edit and Delete buttons.
- [ ] An unknown book id, or a book that is not mine, shows the 404 page.

### US-21: Edit a book
As a user, I want to edit a book, so that I can fix mistakes, update my current page, or change status.
**Covers:** FR-41, FR-42, FR-45
**Acceptance criteria**
- [ ] The edit form is pre-filled with the current values.
- [ ] Saving updates the book and sends me to its page.
- [ ] Current page cannot be more than total pages.
- [ ] Changing the status follows the page and date rules of US-19. Moving back to want to read clears the dates.
- [ ] Invalid input is rejected with a clear error.

### US-22: Delete a book
As a user, I want to delete a book, so that I can remove what I no longer need.
**Covers:** FR-46
**Acceptance criteria**
- [ ] A confirmation appears before delete. Cancel keeps the book.
- [ ] Confirm deletes the book, all its notes, and its cover on Cloudinary.
- [ ] After delete, I land on the library, and a message shows.
- [ ] If I deleted the last book on a page, I see the last valid page.

### US-23: Books are private
As a user, I should only touch my own books, so that my library stays private.
**Covers:** FR-47
**Acceptance criteria**
- [ ] Opening another user's book URL shows the 404 page.
- [ ] Edit and delete on another user's book are blocked.
- [ ] The library shows only my books.
- [ ] The owner of a book is set by the server, never by the form.

---

## Epic 8: Cover image

### US-24: Add or replace a cover
As a user, I want to add a cover image, so that my library looks nice.
**Covers:** FR-48, FR-49, FR-50
**Acceptance criteria**
- [ ] Cover upload is optional on add and edit, and shows a preview.
- [ ] Only JPG, PNG, or WEBP up to 2 MB is accepted. Anything else shows a clear error.
- [ ] A new cover replaces the old one, and the old file is deleted on Cloudinary.
- [ ] A book without a cover shows a placeholder.
- [ ] Covers are served in a reduced size.

---

## Epic 9: Notes

### US-25: Add a note
As a user, I want to add notes to a book, so that I keep my thoughts and quotes.
**Covers:** FR-51, FR-52
**Acceptance criteria**
- [ ] A note has text and an optional page number.
- [ ] The page number cannot be more than the book's total pages.
- [ ] A book can have many notes. Each shows its created date, newest first.
- [ ] Empty text is rejected.
- [ ] A message shows after the note is added.

### US-26: Edit a note
As a user, I want to edit a note, so that I can fix or improve it.
**Covers:** FR-52, FR-53
**Acceptance criteria**
- [ ] The edit form is pre-filled.
- [ ] Saving updates the text and page, with the same rules as adding a note.
- [ ] An edited note shows an "edited" label.

### US-27: Delete a note
As a user, I want to delete a note, so that I can remove what I do not need.
**Covers:** FR-53
**Acceptance criteria**
- [ ] A confirmation appears before delete. Cancel keeps the note.
- [ ] Only that note is removed.

### US-28: Notes are private
As a user, I should only see my own notes, so that my thoughts stay private.
**Covers:** FR-54
**Acceptance criteria**
- [ ] Another user cannot view, edit, or delete my notes by URL.
- [ ] A note must belong to both me and the book in the URL.

---

## Epic 10: Profile

### US-29: Navbar for logged-in users
As a user, I want a navbar with my main places, so that I can move around without a sidebar.
**Covers:** FR-55
**Acceptance criteria**
- [ ] The navbar shows My Library (to `/books`), Add Book, my avatar and username (to `/profile`), and Logout.
- [ ] The current page is highlighted.
- [ ] The avatar shows my image, or the first letter of my username.
- [ ] On mobile, a hamburger menu holds the same items.
- [ ] Visitors see Login and Register instead.
- [ ] There is no sidebar.

### US-30: View profile
As a user, I want a profile page, so that I can see my details and my reading numbers.
**Covers:** FR-56, FR-57, FR-65
**Acceptance criteria**
- [ ] The profile shows avatar (or first letter), username, email, member since, and sign-in method.
- [ ] The profile shows total books, books per status, pages read, and total notes.
- [ ] Counts include only my own data. Zero counts show 0.
- [ ] Each status card opens the library filtered by that status.
- [ ] The profile has an Edit profile button and a link to my library.
- [ ] The profile has a Security section with a Change password button (Set password for a Google-only account).
- [ ] The URL has no id. I can only open my own profile.

### US-31: Edit username
As a user, I want to change my username, so that it says what I want.
**Covers:** FR-58
**Acceptance criteria**
- [ ] The edit page has the username field pre-filled.
- [ ] The username follows the same rule as register. A taken username shows a clear error.
- [ ] Saving updates the username in the navbar and on the profile.
- [ ] A Google user can change the generated username.

### US-32: Profile image
As a user, I want a profile image, so that my account feels like mine.
**Covers:** FR-59
**Acceptance criteria**
- [ ] The image can be added at register (optional) and changed anytime on the edit profile page.
- [ ] The edit page has an image upload with a preview.
- [ ] Only JPG, PNG, or WEBP up to 2 MB is accepted. Anything else shows a clear error.
- [ ] A new image replaces the old one, and the old file is deleted on Cloudinary.
- [ ] A Remove button deletes the image and shows the first letter again.
- [ ] The navbar and the profile show the new image.

### US-33: Change password
As a user, I want to change my password, so that I can keep my account safe.
**Covers:** FR-60, FR-61, FR-62, FR-64
**Acceptance criteria**
- [ ] Change password opens from the Security section on the profile.
- [ ] The form asks for the current password, new password, and confirm password.
- [ ] A wrong current password shows a clear error.
- [ ] The new password follows the password rule, matches the confirm field, and differs from the current one.
- [ ] After saving, the password is stored hashed, and a success message shows on the profile.
- [ ] Other devices are logged out. This device stays logged in.
- [ ] An email tells me the password was changed.
- [ ] Repeated wrong attempts are rate limited.

### US-34: Set a password for a Google-only account
As a Google user, I want to set a password, so that I can also log in with username and password.
**Covers:** FR-63, FR-64
**Acceptance criteria**
- [ ] The Security section shows Set password when the account has no password.
- [ ] The form asks only for a new password and a confirm password.
- [ ] After saving, I can log in with my username and password, and a success message shows.
- [ ] An email tells me a password was set.

---

## Epic 11: Feedback and validation

### US-35: Form validation
As a user, I want clear errors on bad input, so that I know how to fix it.
**Covers:** FR-66, FR-67
**Acceptance criteria**
- [ ] Forms are checked on the client (JavaScript) and on the server (Joi).
- [ ] An error names the field and the problem.
- [ ] Entered values are kept after an error. Password fields are never filled back.
- [ ] An error under a field disappears as soon as I start typing in that field.
- [ ] If the value is still invalid on submit, the error shows again.

### US-36: Action messages
As a user, I want a message after each action, so that I know whether it worked.
**Covers:** FR-68
**Acceptance criteria**
- [ ] A success message shows after register, verify, login, logout, add, edit, delete, and password changes.
- [ ] An error message shows when an action fails.
- [ ] A message can be closed, and shows only once.
- [ ] Messages are announced to screen readers.

### US-37: Error pages
As a user, I want a clear page when something goes wrong, so that I know what to do next.
**Covers:** FR-69
**Acceptance criteria**
- [ ] An unknown URL shows the 404 page with a Back to home button.
- [ ] A book, note, or page that is not mine shows the 404 page.
- [ ] A server error shows the 500 page with no technical details.

---

## Traceability: requirement to story

| Requirement | Story |
|---|---|
| FR-1 to FR-3 | US-1 |
| FR-4 to FR-7 | US-2 |
| FR-8 to FR-10 | US-3 |
| FR-11 | US-4 |
| FR-12 | US-5 |
| FR-13, FR-14, FR-16 | US-6 |
| FR-15 | US-7 |
| FR-17 | US-8 |
| FR-18 | US-9 |
| FR-19, FR-20, FR-22 | US-10 |
| FR-21, FR-23 | US-11 |
| FR-24, FR-25, FR-27 | US-12 |
| FR-26 | US-13 |
| FR-28 | US-14 |
| FR-29, FR-37 | US-15 |
| FR-30, FR-31 | US-16 |
| FR-32 | US-17 |
| FR-33, FR-36 | US-18 |
| FR-34, FR-35 | US-17, US-18 |
| FR-38 to FR-42 | US-19 |
| FR-43 | US-15, US-20 |
| FR-44 | US-20 |
| FR-45 | US-21 |
| FR-46 | US-22 |
| FR-47 | US-23 |
| FR-48 to FR-50 | US-24 |
| FR-51, FR-52 | US-25 |
| FR-53 | US-26, US-27 |
| FR-54 | US-28 |
| FR-55 | US-29 |
| FR-56, FR-57, FR-65 | US-30 |
| FR-58 | US-31 |
| FR-59 | US-32 |
| FR-60 to FR-62, FR-64 | US-33 |
| FR-63 | US-34 |
| FR-66, FR-67 | US-35 |
| FR-68 | US-36 |
| FR-69 | US-37 |