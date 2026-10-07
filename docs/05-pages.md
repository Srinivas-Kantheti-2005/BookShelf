# Pages: BookShelf v1

**Source:** docs/02-user-stories.md, docs/04-routes.md
**Views:** EJS, plain CSS, plain JavaScript
**Date:** 7 Oct 2026

## 1. Page list

| # | Page | URL | Who | Story |
|---|---|---|---|---|
| 1 | Landing | `/` | Visitor | US-20 |
| 2 | Register | `/register` | Visitor | US-1, US-24 |
| 3 | Verify email | `/verify-email` | Visitor | US-21, US-22 |
| 4 | Login | `/login` | Visitor | US-2, US-23, US-24 |
| 5 | Forgot password | `/forgot-password` | Visitor | US-26 |
| 6 | Reset password | `/reset-password` | Visitor | US-26 |
| 7 | Library (paginated) | `/books` | User | US-6, US-16, US-17 |
| 8 | Add book | `/books/new` | User | US-5, US-11 |
| 9 | Book details | `/books/:id` | User | US-7, US-12, US-14 |
| 10 | Edit book | `/books/:id/edit` | User | US-8, US-11 |
| 11 | Edit note | `/books/:id/notes/:noteId/edit` | User | US-13 |
| 12 | 404 not found | any bad URL | Everyone | |
| 13 | 500 error | server error | Everyone | |

## 2. Shared parts (partials)

- **Head:** meta, title, CSS link, viewport tag.
- **Navbar:**
  - Visitor: logo, Login, Register.
  - User: logo, My Library, Add Book, username, Logout (a POST form button).
  - Phone: menu collapses into a hamburger.
- **Flash messages:** success (green) and error (red), closable, shown under the navbar (US-19).
- **Footer:** app name and year.
- **Form error list:** field name and problem under the field, entered values kept (US-18). An error disappears as soon as the user starts typing in that field.
- **Pagination bar:** Previous, page numbers, Next. Used on the Library page. Built as one partial so any list can reuse it later.

## 3. Wireframes

### 3.1 Landing `/`

```
+--------------------------------------------------+
| BookShelf                        [Login][Register]|
+--------------------------------------------------+
|                                                  |
|        Your personal reading library             |
|   Track what you want to read, are reading,      |
|   and finished. Keep notes on every book.        |
|                                                  |
|         [ Get started ]   [ Login ]              |
|                                                  |
+--------------------------------------------------+
|  [Track status]   [Save notes]    [Add covers]   |
|  3 simple stat    page by page    keep it nice   |
|  tracking         thoughts                       |
+--------------------------------------------------+
| BookShelf 2026                                   |
+--------------------------------------------------+
```

### 3.2 Register `/register`

```
+--------------------------------------------------+
| Navbar                                           |
+--------------------------------------------------+
|              Create your account                 |
|          +----------------------------+          |
|          | Username  [______________] |          |
|          | Email     [______________] |          |
|          | Password  [______________] |          |
|          | Confirm   [______________] |          |
|          |                            |          |
|          | [      Register         ]  |          |
|          |          or                |          |
|          | [ Continue with Google  ]  |          |
|          |                            |          |
|          | Have an account? Login     |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- Password hint under the field: "At least 8 characters, with a letter and a number."
- Confirm field shows an error under it when it does not match.

### 3.3 Verify email `/verify-email`

```
+--------------------------------------------------+
|                 Check your email                 |
|          +----------------------------+          |
|          | We sent a 6 digit code to  |          |
|          | s***@mail.com              |          |
|          |                            |          |
|          | Code  [ _ _ _ _ _ _ ]      |          |
|          | [        Verify         ]  |          |
|          |                            |          |
|          | Code expires in 10 minutes |          |
|          | Resend code (wait 60 s)    |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

### 3.4 Login `/login`

```
+--------------------------------------------------+
|                  Welcome back                    |
|          +----------------------------+          |
|          | Email     [______________] |          |
|          | Password  [______________] |          |
|          |                            |          |
|          | [ ] Remember me   Forgot   |          |
|          |                   password?|          |
|          | [        Login          ]  |          |
|          |          or                |          |
|          | [ Continue with Google  ]  |          |
|          | New here? Register         |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- Remember me checkbox is on the left, Forgot password link on the right of the same row.
- Forgot password link goes to `/forgot-password`.
- Unchecked Remember me: session ends when the browser closes. Checked: 30 days.

### 3.4.1 Forgot password `/forgot-password`

```
+--------------------------------------------------+
|              Forgot your password?               |
|          +----------------------------+          |
|          | Enter your email and we    |          |
|          | will send you a code.      |          |
|          |                            |          |
|          | Email  [______________]    |          |
|          | [      Send code        ]  |          |
|          |                            |          |
|          | Back to login              |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- After submit, same message every time: "If an account exists, we sent a code." Then redirect to `/reset-password`.
- This stops people from guessing which emails are registered.

### 3.4.2 Reset password `/reset-password`

```
+--------------------------------------------------+
|                Reset your password               |
|          +----------------------------+          |
|          | Code          [ _ _ _ _ _ _ ]        |
|          | New password  [______________]       |
|          | Confirm       [______________]       |
|          | [   Reset password      ]  |          |
|          |                            |          |
|          | Code expires in 10 minutes |          |
|          | Resend code (wait 60 s)    |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- No pending email cookie: redirect to `/forgot-password`.
- Success: password saved hashed, old code cleared, redirect to `/login` with a success message.
- Wrong code shows a clear error. After 5 wrong attempts the user must request a new code.

### 3.5 Library `/books` (paginated)

```
+--------------------------------------------------+
| BookShelf   My Library  Add Book  srinivas Logout|
+--------------------------------------------------+
| [ Search title or author... ]                    |
| Status [All v]   Genre [All v]           [Clear] |
+--------------------------------------------------+
| My Library (30)                    [+ Add Book]  |
| Showing 1 to 12 of 30 books                      |
|                                                  |
| +----------+  +----------+  +----------+         |
| | [cover]  |  | [cover]  |  | [cover]  |         |
| | Title    |  | Title    |  | Title    |         |
| | Author   |  | Author   |  | Author   |         |
| | genre    |  | genre    |  | genre    |         |
| | READING  |  | FINISHED |  | WANT     |         |
| | [####--] |  | [######] |  | [------] |         |
| | 42%      |  | 100%     |  | 0%       |         |
| +----------+  +----------+  +----------+         |
|   ... 12 cards per page ...                      |
|                                                  |
|     [< Prev]  [1] 2  3  [Next >]                |
+--------------------------------------------------+
```

- Grid: 3 columns desktop, 2 tablet, 1 phone.
- Status badge colors: want to read (grey), reading (blue), finished (green).
- Empty library: message "No books yet" plus an Add Book button. No pagination bar.
- No search match: "Nothing found" plus a Clear link. No pagination bar.
- The count in the heading is the total number of matching books, not only this page.

### 3.5.1 Pagination bar

```
Few pages:    [< Prev]  1  [2]  3  [Next >]

Many pages:   [< Prev]  1  ...  4  [5]  6  ...  20  [Next >]

First page:    < Prev (disabled)  [1]  2  3  [Next >]

Last page:     [< Prev]  1  2  [3]  Next > (disabled)
```

### 3.5.2 Dynamic search and filter

- No Apply button. Results update on their own.
- Search: updates about 400 ms after the user stops typing.
- Status and genre: update the moment a value is picked.
- Only the book cards, count, and pagination bar change. The search and filter fields are not re-rendered, so text and focus stay.
- Any change goes back to page 1.
- URL updates with the values (`/books?q=atomic&status=reading`), so refresh, Back, and bookmarks work.
- Clear button empties all fields and shows the full library.
- Fast typing: an older request that comes back late is ignored, so results never show out of order.
- Small "Loading" state on the list while results load.
- JavaScript off: press Enter in the search box and the form still works as a normal GET.

**Rules**
- Page size: 12 books per page. Fixed in v1, not user changeable.
- Sort order: newest added first (`createdAt` descending), same on every page.
- Current page is highlighted and marked `aria-current="page"`.
- Prev is disabled on page 1. Next is disabled on the last page.
- Many pages: always show first page, last page, current page, and 1 neighbour each side. Gaps show as `...`.
- Page numbers are normal links, so Back button, bookmarks, and sharing work.
- Bar is hidden when there is only 1 page.
- Phone: show only Prev, current page "Page 2 of 5", Next, so it fits the screen.
- Bar sits below the cards.
- Page number goes in the URL: `/books?page=2`.
- Search and filters stay in every pagination link, so `/books?q=atomic&status=reading&page=2` keeps working.
- Applying a search or filter always goes back to page 1.
- Page number that is not valid (0, negative, text) shows page 1.
- Page number above the last page shows the last page.
- After the last book on a page is deleted, user lands on the library and sees the last valid page.
- Changing pages scrolls to the top of the list.

### 3.6 Add book `/books/new` and Edit book `/books/:id/edit`

```
+--------------------------------------------------+
|                 Add a book                       |
|   Title        [________________________]        |
|   Author       [________________________]        |
|   Genre        [ Select genre         v ]        |
|   Status       [ Want to read         v ]        |
|   Total pages  [______]  Current page [______]    |
|   Description  [________________________]        |
|                [________________________]        |
|   Cover        [ Choose file ]  (jpg png webp)   |
|                [ preview ]                       |
|                                                  |
|   [ Save ]  [ Cancel ]                           |
+--------------------------------------------------+
```

- Edit uses the same form, pre-filled, with a cover preview and a button to replace it.
- Choosing Want to read sets current page 0 and disables the field.
- Choosing Finished sets current page to total pages and disables the field.
- Client JS checks current page is not more than total pages.

### 3.7 Book details `/books/:id`

```
+--------------------------------------------------+
| < Back to library                                |
|                                                  |
| +--------+  Title                                |
| | [cover]|  by Author                            |
| |        |  genre   READING                      |
| +--------+  Pages 128 / 300   [########----] 42% |
|             Started 1 Oct 2026                   |
|             [ Edit ]  [ Delete ]                 |
|                                                  |
| Description                                      |
| ------------------------------------------------ |
|                                                  |
| Notes (3)                                        |
| +--------------------------------------------+   |
| | Add a note                                 |   |
| | [ text area ]          Page [____]         |   |
| | [ Add note ]                               |   |
| +--------------------------------------------+   |
| +--------------------------------------------+   |
| | Small habits compound.                     |   |
| | p. 42 | 5 Oct 2026 (edited)  [Edit][Delete]|   |
| +--------------------------------------------+   |
+--------------------------------------------------+
```

- Delete book and Delete note ask for confirmation first (small modal or `confirm()`).
- Notes list newest first. Empty notes: "No notes yet".
- Finished date shows when finished.
- Notes list is not paginated in v1.

### 3.8 Edit note `/books/:id/notes/:noteId/edit`

```
+--------------------------------------------------+
| < Back to book                                   |
|   Edit note                                      |
|   Text [ pre-filled text area ]                  |
|   Page [____]                                    |
|   [ Save ]  [ Cancel ]                           |
+--------------------------------------------------+
```

### 3.9 Error pages

```
+--------------------------------------------------+
|                      404                         |
|            This page was not found.              |
|             [ Back to home ]                     |
+--------------------------------------------------+
```

500 page: same layout, message "Something went wrong. Try again later." No error details shown.

## 4. Look and feel

- Style: clean, library card catalog feel.
- Colors: warm paper background, dark brown text, one accent color.
- Fonts: serif for headings, sans-serif for body.
- Responsive: mobile first, breakpoints near 600px and 900px.
- Form fields show a label and an error message under the field.
- Status badge colors as in 3.5.
- Pagination bar: current page filled with the accent color, other pages plain links, disabled Prev or Next greyed out.

## 5. Client side JavaScript

- Form checks before submit: required fields, current page not over total pages.
- Status change updates the current page field (0 or equal to total pages).
- Cover file: type and size check, image preview.
- Flash messages close button.
- Delete confirmation.
- Verify page: resend button countdown from 60 seconds.
- Reset password page: password match check, resend button countdown from 60 seconds.
- Navbar hamburger on phone.
- Pagination needs no JavaScript. Plain links, the server renders each page.
- Register page: password rule check and password match check.
- Form errors: an error under a field is removed when the user types in that field. If the value is still invalid on submit, the error shows again.
- Library: search with 400 ms delay, status and genre on change, list swapped without page reload, URL updated, late responses ignored.
- Library: Clear button resets all fields and reloads the full list.