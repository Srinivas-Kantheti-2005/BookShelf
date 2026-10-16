# Pages: BookShelf v1

**Source:** docs/01-prd.md, docs/02-user-stories.md, docs/04-routes.md, docs/08-design-system.md
**Views:** EJS, plain CSS, plain JavaScript
**Date:** 16 Oct 2026

## 1. Page list

| # | Page | URL | Access | H1 | Stories |
|---|---|---|---|---|---|
| 1 | Landing | `/` | Public | Your personal reading library | US-1 |
| 2 | Register | `/register` | Guest | Create your account | US-2 |
| 3 | Verify email | `/verify-email` | Pending | Check your email | US-3, US-4 |
| 4 | Login | `/login` | Guest | Welcome back | US-6, US-7 |
| 5 | Forgot password | `/forgot-password` | Guest | Forgot your password? | US-10 |
| 6 | Reset password | `/reset-password` | Pending | Reset your password | US-11 |
| 7 | Library | `/books` | Login | My library | US-15, US-16, US-17, US-18 |
| 8 | Add book | `/books/new` | Login | Add a book | US-19, US-24 |
| 9 | Book details | `/books/:id` | Login | The book title | US-20, US-22, US-25, US-27 |
| 10 | Edit book | `/books/:id/edit` | Login | Edit book | US-21, US-24 |
| 11 | Edit note | `/books/:id/notes/:noteId/edit` | Login | Edit note | US-26 |
| 12 | Profile | `/profile` | Login | Profile | US-30 |
| 13 | Edit profile | `/profile/edit` | Login | Edit profile | US-31, US-32 |
| 14 | Change password | `/profile/password` | Login | Change password (Set a password for a Google-only account) | US-33, US-34 |
| 15 | Not found | any unknown URL | Public | This page was not found. | US-37 |
| 16 | Server error | on a server error | Public | Something went wrong. Try again later. | US-37 |

The navbar (US-29), flash messages (US-36), and form validation (US-35) are shared parts, described in section 3.

## 2. Devices and layout

| | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Width | up to 599 px | 600 to 899 px | 900 px and up |
| Designed at | 390 | 768 | 1440 |
| Figma frame | `Mobile/...` | `Tablet/...` | `Desktop/...` |
| Grid | 4 columns, gutter 16, margin 16 | 8 columns, gutter 24, margin 32 | 12 columns, gutter 24, margin 160, content max 1120 |
| Page padding, top and bottom | 24 | 32 | 48 |
| Headings | `Display Mobile`, `H1 Mobile` | desktop sizes | desktop sizes |
| Navbar | height 56, hamburger menu | height 64, full links | height 64, full links |
| Auth card | width Fill, padding 24, no shadow | width 420, centered, padding 32, shadow | same as tablet |
| Library grid | 1 column, horizontal cards | 2 columns, vertical cards | 3 columns, vertical cards |
| Pagination | Prev, `PAGE 2 OF 3`, Next | page numbers | page numbers |
| Modal buttons | stacked, Fill | right aligned | right aligned |

- Build mobile first. CSS base styles are for phones, then `min-width: 600px` for tablet and `min-width: 900px` for laptop and PC.
- No horizontal scrolling from 320 px up. Touch targets are at least 44 px.
- Long text (description, notes) is at most 720 px wide, so lines stay at 60 to 75 characters.
- Copy style: only the first word of a button, link, label, or message is capitalized.

## 3. Shared parts

### 3.1 Page structure

- Landmarks: `header` (navbar), `main`, `footer`. One H1 per page, inside `main`.
- First focusable item is a "Skip to content" link, visible on focus.
- The flash area sits under the navbar, above the page header.

### 3.2 Navbar

Visitor, tablet and laptop:

```
+--------------------------------------------------+
| BookShelf                       Log in  [Register] |
+--------------------------------------------------+
```

Logged in, tablet and laptop:

```
+------------------------------------------------------------+
| BookShelf  My Library  Add Book        (S) srinivas [Logout] |
+------------------------------------------------------------+
```

Mobile, closed and open (logged in):

```
+----------------------+     +----------------------+
| BookShelf        [=] |     | BookShelf        [x] |
+----------------------+     +----------------------+
                             | My Library           |
                             | Add Book             |
                             | (S) srinivas         |
                             | [      Logout      ] |
                             +----------------------+
```

- The mobile visitor menu has `Log in` and `Register`.
- The current page is highlighted: My Library on `/books` and book pages, Add Book on `/books/new`, the avatar and username on `/profile` pages.
- The mobile menu closes on Esc, on choosing a link, and when the window grows past 600 px. The hamburger has `aria-expanded`.
- Logout is a form button that posts to `/logout`.

### 3.3 Other shared parts

| Part | Description |
|---|---|
| Flash message | Alert under the navbar. Success, info, warning, or error, with a close button. Mobile: width Fill with 16 margin. Tablet and laptop: max 640, centered. Shown once |
| Footer | `BookShelf 2026`, centered |
| Form field | Label above, field, then a hint or an error under it. The error replaces the hint, names the field and the problem, and disappears when the user types in the field |
| Avatar | Image in a circle, or the first letter of the username on a tinted circle. Small in the navbar, medium on register, large on the profile |
| Page header | H1, optional subtitle, optional primary button. Laptop and tablet: one row. Mobile: stacked, button Fill |
| Pagination bar | Prev, numbers, Next. Used on the library only. Hidden when there is one page |
| Confirm modal | Delete confirmation. Esc and Cancel close it, focus stays inside while it is open, and focus returns to the button that opened it |
| Empty state | Art, a title, one line, and an optional button |
| Loading | A button shows a spinner and `Saving...`. The library list dims and the search icon becomes a spinner |

## 4. Pages

### 4.1 Landing `/`

```
+--------------------------------------------------+
| Navbar (visitor)                                 |
+--------------------------------------------------+
|                                                  |
|          Your personal reading library           |
|     Track what you want to read, are reading,    |
|     and have finished. Keep notes on every book. |
|                                                  |
|          [ Get started ]    [ Log in ]           |
|                                                  |
+--------------------------------------------------+
| Track every book | Keep your notes | Find fast   |
| [icon]           | [icon]          | [icon]      |
| Status, pages,   | Thoughts and    | Search and  |
| and progress     | quotes by page  | filters     |
+--------------------------------------------------+
| Footer                                           |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Hero | Stacked, left aligned. `Display Mobile` | Centered | Centered, text max 560 |
| Buttons | Stacked, Fill. Get started on top | Side by side | Side by side |
| Features | 3 cards stacked | 3 cards in a row | 3 cards in a row |

- `Get started` goes to `/register`. `Log in` goes to `/login`.
- The three features use `Card/Feature` with the icons book, edit, and search.
- A logged-in user is sent to `/books` before this page shows.

### 4.2 Register `/register`

```
+--------------------------------------------------+
| Navbar (visitor)                                 |
+--------------------------------------------------+
|              Create your account                 |
|          +----------------------------+          |
|          | (  S  )  [ Add photo ]     |          |
|          | Optional. JPG, PNG or WEBP, max 2 MB  |
|          |                            |          |
|          | Username  [______________] |          |
|          | Email     [______________] |          |
|          | Password  [______________] (eye)      |
|          | Confirm   [______________] (eye)      |
|          |                            |          |
|          | [       Register        ]  |          |
|          |            or              |          |
|          | [ Continue with Google  ]  |          |
|          |                            |          |
|          | Have an account? Log in    |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Card | Width Fill, no shadow | 420, centered, shadow | 420, centered, shadow |
| Photo row | Compact: circle 64 and `Add photo` button | same | same |

- The circle shows the first letter of the username as the user types. Choosing a file shows the preview, and a `Remove` link appears.
- Hints: username `3 to 30 characters: lowercase letters, numbers, and underscore`. Password `At least 8 characters, with a letter and a number`.
- Errors: taken username, email already used, password rule, confirm does not match, file type or size.
- Button loading text: `Creating account...`. After success the user goes to `/verify-email`.

### 4.3 Verify email `/verify-email`

```
+--------------------------------------------------+
|                 Check your email                 |
|          +----------------------------+          |
|          | We sent a 6 digit code to  |          |
|          | s***@mail.com              |          |
|          |                            |          |
|          | Verification code          |          |
|          | [ ][ ][ ][ ][ ][ ]         |          |
|          | Enter the 6 digit code     |          |
|          |                            |          |
|          | [        Verify         ]  |          |
|          |                            |          |
|          | Code expires in 10 minutes |          |
|          | Resend code (wait 60 s)    |          |
|          | Wrong email? Register again|          |
|          +----------------------------+          |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Card | Width Fill | 420, centered | 420, centered |
| Boxes | 6 boxes share the card width, about 45 wide | about 53 wide | about 53 wide |

- Typing a digit moves to the next box. Backspace goes back. Pasting a code fills all six. The first box has focus on load.
- Error: `Code is not correct`. All six boxes turn red. After 5 wrong attempts the message says to request a new code.
- `Resend code` is disabled with a countdown from 60 seconds, then active.
- The masked email comes from the pending email in the session.

### 4.4 Login `/login`

```
+--------------------------------------------------+
|                  Welcome back                    |
|          +----------------------------+          |
|          | Log in to continue to your library    |
|          |                            |          |
|          | Username  [______________] |          |
|          | Password  [______________] (eye)      |
|          |                            |          |
|          | [ ] Remember me  Forgot password?     |
|          | [        Log in         ]  |          |
|          |            or              |          |
|          | [ Continue with Google  ]  |          |
|          | New here? Register         |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- Layout by device is the same as Register (card Fill on mobile, 420 centered on tablet and laptop).
- The checkbox is on the left and the link on the right of one row. On very narrow screens they wrap onto two rows.
- Error: one message above the form, `Username or password is wrong`. An unverified user is sent to the verify page.

### 4.5 Forgot password `/forgot-password`

```
+--------------------------------------------------+
|              Forgot your password?               |
|          +----------------------------+          |
|          | Enter your email and we    |          |
|          | will send you a code.      |          |
|          |                            |          |
|          | Email  [______________]    |          |
|          | [       Send code       ]  |          |
|          | Back to login              |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- Layout by device is the same as Register.
- After submit the user always sees `If an account exists, we sent a code.` and goes to `/reset-password`.

### 4.6 Reset password `/reset-password`

```
+--------------------------------------------------+
|                Reset your password               |
|          +----------------------------+          |
|          | Verification code          |          |
|          | [ ][ ][ ][ ][ ][ ]         |          |
|          | New password  [__________] (eye)      |
|          | Confirm       [__________] (eye)      |
|          | At least 8 characters, with a letter and a number |
|          | [    Reset password     ]  |          |
|          | Code expires in 10 minutes |          |
|          | Resend code (wait 60 s)    |          |
|          +----------------------------+          |
+--------------------------------------------------+
```

- Layout by device is the same as Register. Code boxes behave as on the verify page.
- Success: the user goes to `/login` with a success message.

### 4.7 Library `/books`

Laptop and PC:

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| My library                         [+ Add book]  |
| 30 books                                         |
|                                                  |
| [ Search title or author...  ] [Status v] [Genre v] Clear |
|                                                  |
| Showing 1 to 12 of 30 books                      |
| +----------+  +----------+  +----------+         |
| | [cover]  |  | [cover]  |  | [cover]  |         |
| | Title    |  | Title    |  | Title    |         |
| | Author   |  | Author   |  | Author   |         |
| | GENRE    |  | GENRE    |  | GENRE    |         |
| | (READING)|  |(FINISHED)|  |(WANT TO READ)|     |
| | [###---] |  | [######] |  | [------] |         |
| +----------+  +----------+  +----------+         |
|            ... 12 cards per page ...             |
|                                                  |
|          [< Prev]  1  [2]  3  [Next >]          |
+--------------------------------------------------+
```

Mobile:

```
+----------------------+
| BookShelf        [=] |
+----------------------+
| My library           |
| 30 books             |
| [+ Add book]         |
| [ Search...        ] |
| [Status v] [Genre v] |
| Clear                |
| Showing 1 to 12 of 30|
| +------------------+ |
| |[cov] Title       | |
| |      Author      | |
| |      GENRE       | |
| |      (READING)   | |
| |      [###--] 42% | |
| +------------------+ |
|    ... 12 cards ...  |
| [< Prev] PAGE 2 OF 3 [Next >] |
+----------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Page header | Stacked, Add book Fill | One row | One row |
| Search and filters | Search, then two selects side by side, then Clear | Search on row 1. Status, genre, and Clear on row 2 | One row |
| Cards | 1 column, horizontal (cover 88 at the left) | 2 columns, vertical | 3 columns, vertical |
| Pagination | Prev, `PAGE 2 OF 3`, Next | Prev, numbers, Next | Prev, numbers, Next |

- The count under the title is the number of matching books. With a filter it reads `8 books found`.
- A card opens the book details. The progress bar shows the percentage. The status pill always has its text.
- Search and filters update the list on their own, with no Apply button (see 6). The URL updates.
- Empty library: empty state `No books yet`, `Add your first book to start your library.`, and a primary `Add a book` button. No pagination bar.
- No match: empty state `Nothing found`, `Try a different search or clear the filters.`, and an outline `Clear filters` button.
- Loading: the cards dim, and the search icon becomes a spinner.

### 4.8 Add book `/books/new`

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| < Back to library                                |
| Add a book                                       |
| +----------------------------------------------+ |
| | Cover            Title    [______________]   | |
| | +----------+     Author   [______________]   | |
| | | [ upload]|     Genre [Select v]  Status [Want to read v] | |
| | | JPG, PNG |     Total pages [___] Current page [___]      | |
| | | max 2 MB |     Description                | |
| | +----------+     [___________________]       | |
| |                  [___________________]       | |
| |                            [Cancel] [Save]   | |
| +----------------------------------------------+ |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Form card | Width Fill, padding 24 | Width Fill, padding 32 | Max 880, centered, padding 32 |
| Layout | One column. Cover upload first | Two columns: cover 200 at the left, fields at the right | Two columns: cover 240 at the left, fields at the right |
| Field rows | Every field on its own row | Genre and status share a row. Total and current pages share a row | same as tablet |
| Buttons | Stacked, Fill, Save on top | Right aligned, Cancel then Save | Right aligned, Cancel then Save |

- Required: title, author, genre, total pages, status. Description is optional (counter `0 / 2000`).
- Choosing `Want to read` sets the current page to 0 and disables it. Choosing `Finished` sets it to the total pages and disables it. A current page above the total shows an error.
- The cover shows a preview. Error text: `Choose a JPG, PNG, or WEBP image up to 2 MB`.
- Save loading text: `Saving...`. Success goes to `/books` with `Book added.`

### 4.9 Book details `/books/:id`

Laptop and PC:

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| < Back to library                                |
|                                                  |
| +--------+  Atomic Habits                        |
| |        |  by James Clear                       |
| | [cover]|  SELF-HELP   (READING)                |
| |        |  Pages 128 / 300   [########----] 42% |
| +--------+  Started 1 Oct 2026                   |
|             [ Edit ]  [ Delete ]                 |
|                                                  |
| Description                                      |
| A short description of the book...               |
|                                                  |
| Notes (3)                                        |
| +--------------------------------------------+   |
| | Add a note                                 |   |
| | [ text area ]            Page [____]       |   |
| | 0 / 2000                 [ Add note ]      |   |
| +--------------------------------------------+   |
| +--------------------------------------------+   |
| | Small habits compound.                [edit][del] |
| | PAGE 42  5 Oct 2026  edited                |   |
| +--------------------------------------------+   |
+--------------------------------------------------+
```

Mobile:

```
+----------------------+
| BookShelf        [=] |
+----------------------+
| < Back to library    |
|      +--------+      |
|      | [cover]|      |
|      +--------+      |
| Atomic Habits        |
| by James Clear       |
| SELF-HELP (READING)  |
| Pages 128 / 300      |
| [########----] 42%   |
| Started 1 Oct 2026   |
| [ Edit ]  [ Delete ] |
| Description          |
| ...                  |
| Notes (3)            |
| [ text area ]        |
| Page [____]          |
| [    Add note     ]  |
| (note card)          |
+----------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Header | Stacked and centered. Cover 160 | Row: cover 200 at the left, details at the right | Row: cover 240 at the left, details at the right |
| Edit and Delete | Side by side, each Fill | Hug width, at the end of the details | Hug width, at the end of the details |
| Description and notes | Full width | Max 720 | Max 720 |
| Add note form | Stacked: text area, page (120), button Fill | Page and button share a row | Page and button share a row |

- Delete opens the confirm modal. Title `Delete this book?`, text `This also deletes its notes and its cover. You cannot undo this.`, buttons `Cancel` and `Delete`.
- Notes show newest first, each with the date, the page, and an `edited` label when changed. Each note has edit and delete icon buttons. Deleting a note opens the modal (`Delete this note?`).
- Empty notes: `No notes yet`, `Write your thoughts and quotes as you read.`
- The note page number cannot be more than the book's total pages. The error names the limit.

### 4.10 Edit book `/books/:id/edit`

- Same page, form, and layout by device as Add book (4.8).
- The H1 is `Edit book`. The fields are pre-filled. The cover shows the current image with a `Replace` button and a `Remove` option.
- Save goes to the book details page with `Book updated.`
- Changing the status follows the same page and date rules. Moving back to `Want to read` clears the dates.

### 4.11 Edit note `/books/:id/notes/:noteId/edit`

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| < Back to book                                   |
|   Edit note                                      |
|   +------------------------------------+         |
|   | Note                               |         |
|   | [ pre-filled text area ]           |         |
|   | Page [____]                        |         |
|   |              [ Cancel ] [ Save ]   |         |
|   +------------------------------------+         |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Card | Width Fill, padding 24 | Max 560, centered, padding 32 | Max 560, centered, padding 32 |
| Buttons | Stacked, Fill, Save on top | Right aligned | Right aligned |

- Save goes to the book details page with `Note updated.`

### 4.12 Profile `/profile`

Laptop and PC:

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| Profile                                          |
|                                                  |
| +------+  srinivas                [Edit profile] |
| |  S   |  srinivas@mail.com                      |
| +------+  Member since Oct 2026                  |
|           Signed in with Google                   |
|                                                  |
| Your library                      [View library] |
| +------+ +------+ +------+ +------+ +------+ +------+ |
| |  30  | |  8   | |  4   | |  18  | | 5,240| |  62  | |
| |Books | |Want  | |Read- | |Fini- | |Pages | |Notes | |
| |      | |to read|ing  | |shed  | |read  | |      | |
| +------+ +------+ +------+ +------+ +------+ +------+ |
|                                                  |
| Security                                         |
| Password                      [Change password]  |
+--------------------------------------------------+
```

Mobile:

```
+----------------------+
| BookShelf        [=] |
+----------------------+
| Profile              |
|      (  S  )         |
| srinivas             |
| srinivas@mail.com    |
| Member since Oct 2026|
| Signed in with Google|
| [   Edit profile   ] |
| Your library         |
| [ 30 ] [  8 ]        |
| [  4 ] [ 18 ]        |
| [5,240] [ 62 ]       |
| [  View library  ]   |
| Security             |
| Password             |
| [ Change password ]  |
+----------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Header | Stacked, avatar centered. Edit profile Fill | Row: avatar 96 at the left, details, button at the right | same as tablet |
| Number cards | 2 columns, 3 rows | 3 columns, 2 rows | 6 in one row |
| Security | Stacked, button Fill | One row, button at the right | One row, button at the right |

- The sign-in method reads `Signed in with a password`, `Signed in with Google`, or `Signed in with a password or Google`.
- The cards are Books, Want to read, Reading, Finished, Pages read, Notes. The three status cards open the library filtered by that status. Zero shows `0`.
- A Google-only account sees `Set password` instead of `Change password`.

### 4.13 Edit profile `/profile/edit`

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| < Back to profile                                |
|   Edit profile                                   |
|   +------------------------------------+         |
|   | (  S  )  [ Choose photo ] [ Remove ]|        |
|   |          JPG, PNG or WEBP, max 2 MB |        |
|   |                                    |         |
|   | Username  [______________]         |         |
|   | Email     srinivas@mail.com        |         |
|   |           You cannot change this   |         |
|   |            [ Cancel ] [ Save ]     |         |
|   +------------------------------------+         |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Card | Width Fill, padding 24 | Max 560, centered, padding 32 | Max 560, centered, padding 32 |
| Photo | Centered, buttons stacked and Fill | Avatar at the left, buttons and hint at the right | same as tablet |
| Buttons | Stacked, Fill, Save on top | Right aligned | Right aligned |

- Choosing a photo shows a preview in the circle before saving. `Remove` clears it and shows the first letter.
- The email is plain text, not a field.
- Errors: taken username, username rule, file type or size. Save goes to `/profile` with `Profile updated.`

### 4.14 Change password `/profile/password`

```
+--------------------------------------------------+
| Navbar (user)                                    |
+--------------------------------------------------+
| < Back to profile                                |
|   Change password                                |
|   +------------------------------------+         |
|   | Current password  [_________] (eye)|         |
|   | New password      [_________] (eye)|         |
|   | Confirm password  [_________] (eye)|         |
|   | At least 8 characters, with a letter and a number |
|   |         [ Cancel ] [ Save password ]|        |
|   +------------------------------------+         |
+--------------------------------------------------+
```

- Layout by device is the same as Edit profile.
- A Google-only account sees the H1 `Set a password`, no current password field, and the button `Set password`.
- Errors: `Current password is incorrect`, the password rule, `Passwords do not match`, and `Choose a password that is different from your current one`.
- Success goes to `/profile` with `Password changed.` (or `Password set.`).

### 4.15 Not found (404) and 4.16 Server error (500)

```
+--------------------------------------------------+
| Navbar                                           |
+--------------------------------------------------+
|                       404                        |
|            This page was not found.              |
|               [ Back to home ]                   |
+--------------------------------------------------+
| Footer                                           |
+--------------------------------------------------+
```

| Part | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Content | Centered, `Display Mobile` for the number | Centered | Centered |
| Padding | 32 top and bottom | 48 | 48 |

- The 500 page shows `500` and `Something went wrong. Try again later.`, with no technical details.
- The message is the H1 (styled as H2). The number is a paragraph, so the page keeps one H1.
- The navbar matches the user state (visitor or logged in). `Back to home` goes to `/`.

## 5. Figma frames

**Base frames: 16 pages by 3 devices = 48.**

Name: `Mobile/Library`, `Tablet/Library`, `Desktop/Library`, and so on. Design mobile first, then tablet, then laptop and PC.

**State frames (mobile first, then the other devices where the layout differs)**

| Frame | For |
|---|---|
| `Login Error` | Error alert and field errors |
| `Register Error` | Field errors, and the live first letter |
| `Verify Error` | Red boxes and the message |
| `Reset Password` | Code boxes and two password fields |
| `Library Empty` | No books yet |
| `Library No Results` | Nothing found |
| `Library Loading` | Dimmed cards |
| `Add Book Error` | Field errors and the cover error |
| `Book Details No Notes` | Empty notes state |
| `Delete Book Modal` | Confirm modal |
| `Delete Note Modal` | Confirm modal |
| `Menu Visitor` and `Menu User` | Open mobile menu |
| `Profile No Photo` | First letter circle |
| `Set Password` | Google-only variant |
| `Flash Messages` | Success, info, warning, and error examples |

## 6. Client JavaScript by page

| File | Pages | What it does |
|---|---|---|
| `formValidation.js` | All forms | Field checks before submit. Errors clear when the user types. Password rule and match checks |
| `navbar.js` | All | Mobile menu: opens, closes on Esc, link click, and resize. Keeps `aria-expanded` |
| `flash.js` | All | Close button for messages |
| `modal.js` | Book details | Opens and closes the confirm modal. Focus stays inside and returns |
| `library.js` | Library | Search with 400 ms delay, status and genre on change, swaps the results, updates the URL, ignores late responses, Clear button |
| `bookForm.js` | Add and Edit book | Status changes the current page field. Current page limit check. Description counter. Cover type and size check and preview |
| `otpInput.js` | Verify, Reset | Six boxes: move on typing, Backspace goes back, paste fills all, joins the digits into one field |
| `otpTimer.js` | Verify, Reset | Resend countdown from 60 seconds |
| `avatarUpload.js` | Register, Edit profile | Photo type and size check, preview, Remove, and the live first letter on Register |
| Password eye toggle | Register, Login, Reset, Change password | Part of `formValidation.js`. Shows or hides the password |

Pagination needs no JavaScript. The page links are plain links, and the server renders each page.

## 7. Page titles (`<title>`)

| Page | Title |
|---|---|
| Landing | BookShelf: your personal reading library |
| Register | Create your account | BookShelf |
| Verify email | Check your email | BookShelf |
| Login | Log in | BookShelf |
| Forgot, Reset | Forgot password | BookShelf, Reset password | BookShelf |
| Library | My library | BookShelf |
| Add, Edit book | Add a book | BookShelf, Edit book | BookShelf |
| Book details | The book title | BookShelf |
| Edit note | Edit note | BookShelf |
| Profile pages | Profile, Edit profile, Change password, each followed by | BookShelf |
| Errors | Page not found | BookShelf, Something went wrong | BookShelf |

## 8. Requirements covered

| Area | Requirements | Pages |
|---|---|---|
| Landing | FR-1 to FR-3 | 1 |
| Register and verify | FR-4 to FR-12 | 2, 3 |
| Login and session | FR-13 to FR-18 | 4, navbar |
| Forgot and reset | FR-19 to FR-23 | 5, 6 |
| Google | FR-24 to FR-28 | Google buttons on 2 and 4 |
| Library | FR-29 to FR-37 | 7 |
| Books and cover | FR-38 to FR-50 | 8, 9, 10 |
| Notes | FR-51 to FR-54 | 9, 11 |
| Profile | FR-55 to FR-65 | 12, 13, 14, navbar |
| Feedback and validation | FR-66 to FR-69 | Shared parts, 15, 16 |
| Three devices | NFR-1, NFR-2 | Section 2 and every page |