# Design Brief: BookShelf v1

**Source:** docs/00-client-request.md, docs/01-prd.md, docs/05-pages.md
**Details live in:** docs/08-design-system.md
**Date:** 16 Oct 2026

## 1. Purpose

BookShelf is a private reading library. The design should make a reader feel calm and organized, like a quiet library with a card catalog. The reader should always know what they have, what they are reading, and how far they have got.

## 2. Users and devices

- One reader, using the app alone, in short visits: add a book, update the current page, write a note, find a book.
- A reader signs up with email or Google, so the first screens (landing, register, verify, login) matter most.

| Device | Width | Design frame | Typical use |
|---|---|---|---|
| Mobile | up to 599 px | 390 | Updating progress and writing notes while reading |
| Tablet | 600 to 899 px | 768 | Browsing the library, reading notes |
| Laptop and PC | 900 px and up | 1440 | Adding books, writing longer notes |

Design mobile first, then tablet, then laptop and PC. All three devices are designed, so every page has three versions.

## 3. Key tasks to make easy

1. Add a book in under a minute.
2. Update reading progress in two taps.
3. Find a book by search or filter.
4. Write a note while reading.
5. See the whole library, and each status, at a glance.
6. Sign up and verify the email without getting lost.

## 4. Personality

- **Keywords:** calm, warm, bookish, organized, trustworthy.
- **Not:** flashy, corporate, childish, crowded, sterile.
- **Tone of the words:** short, plain, kind. Only the first word of a button, link, label, or message is capitalized (`Forgot password?`, `Save book`). Messages come from the table in `docs/04-routes.md`.

## 5. Visual direction (decided)

| Part | Decision |
|---|---|
| Theme | Library card catalog: warm paper page, white cards with thin borders, soft corners, light shadow |
| Page and cards | Page is warm off-white (`bg`). Cards, inputs, and the modal are white (`surface`) |
| Brand color | Dark navy (`primary`) for buttons, links, focus, and current page |
| Accent | Wine (`secondary`), decoration only: the logo bar, dividers, and empty state art |
| Neutrals | Warm gray for text, borders, and disabled states |
| Status colors | Success, warning, error, and info, each with a light background, a middle color, and a dark text color. Light backgrounds are shade 200 |
| Fonts | Gentium Book Plus (headings and reading text) with Inter (everything you use: buttons, forms, navigation, labels). No mono font |
| Status pills | Fully rounded. Want to read uses the primary tints, Reading uses info, Finished uses success. Always with the text |
| Icons | Lucide, one line style, stroke 2, 20 icons |
| Shape | Radius 4 (inputs), 8 (cards, buttons), full (pills, progress bar) |
| Space | 4, 8, 12, 16, 24, 32, 48, 64 only |
| Imagery | Book covers and profile photos only. No stock photos |
| Motion | Hover, fade, and the loading spinner only |

Exact values (28 color tokens, 13 text styles, spacing, shadows) are in `docs/08-design-system.md`. The colors passed WebAIM contrast tests.

## 6. Screens to design

Every screen is designed on Mobile, Tablet, and Laptop and PC (16 pages, 48 base frames).

| # | Screen | Specific things to design |
|---|---|---|
| 1 | Landing | Hero, two buttons, three feature cards |
| 2 | Register | Compact photo upload with the live first letter, show and hide password, Google button |
| 3 | Verify email | Six code boxes (empty, focused, typing, filled, error), resend countdown |
| 4 | Login | Remember me, Forgot password link, Google button |
| 5 | Forgot password | One field, always the same message |
| 6 | Reset password | Six code boxes plus two password fields |
| 7 | Library | Search and filters, pagination, 1 / 2 / 3 column grid, cards with cover, pill, and progress |
| 8 | Add book | Cover upload, genre and status selects, page fields, description counter |
| 9 | Book details | Cover, progress, notes list, add note form, confirm modals |
| 10 | Edit book | Same as add, pre-filled, with the current cover |
| 11 | Edit note | One text area and a page field |
| 12 | Profile | Avatar, details, six library numbers, Security row |
| 13 | Edit profile | Avatar upload, username, read-only email |
| 14 | Change password | Three password fields (Set a password for a Google-only account) |
| 15 | Not found | Large number, message, Back to home |
| 16 | Server error | Same layout as Not found |

The navbar (visitor and logged in, with the mobile menu), flash messages, and the footer appear on these screens.

### 6.1 State frames

| Frame | Devices |
|---|---|
| Login Error, Register Error, Verify Error | Mobile, then Tablet and Laptop and PC |
| Library Empty, Library No Results, Library Loading | Mobile, Tablet, Laptop and PC |
| Add Book Error | Mobile, Laptop and PC |
| Book Details No Notes | Mobile, Laptop and PC |
| Delete Book Modal, Delete Note Modal | Mobile, Laptop and PC |
| Menu Visitor, Menu User (open mobile menu) | Mobile |
| Profile No Photo | Mobile, Laptop and PC |
| Set Password (Google-only account) | Mobile, Laptop and PC |
| Flash Messages (success, info, warning, error) | Mobile, Laptop and PC |

### 6.2 Component states

- Buttons: default, hover, pressed, focus, disabled, loading.
- Fields: default, hover, focus, filled, error, disabled.
- Code boxes: empty, focus, filled, error, disabled.
- Checkbox: unchecked, checked, focus, disabled.
- Cards: default, hover, focus. A book card also with no cover.
- Progress bar: empty, partial, complete.
- Pagination: first page, middle, last page, and only a few pages.

## 7. Components to design

About 35 components, all in the `Design System` page. The list and the specs are in section 7 of `docs/08-design-system.md`.

| Group | Components |
|---|---|
| Actions | Button, Button/Icon, Link |
| Forms | Input (Text, Password, Select, Textarea, OTP), OTP box, Checkbox, Divider, Cover upload, Form actions |
| Feedback | Alert, Modal, Status pill, Progress bar |
| Navigation | Navbar (visitor, user), Mobile menu, Footer, Page header, Pagination |
| Library and books | Library toolbar, Book card, Book header, Note card, Note form |
| Profile | Avatar, Profile header, Stat card, Avatar upload, Security row |
| Page level | Auth card, Feature card, Empty state, Error state |

Each component is designed once, with all its states, and then reused. A screen never uses a loose copy.

## 8. Brand assets

| Asset | Description | Used in |
|---|---|---|
| Logo symbol | Three books on a shelf line, flat, two or three colors, readable at 24 px. Made with the image prompt, then converted to SVG | Navbar, favicon |
| Wordmark | `BookShelf` in Gentium Book Plus Bold, with a small wine bar before it (until the symbol replaces the bar) | Navbar, emails |
| Favicon | The symbol at 32 px | Browser tab |
| Empty state art | Simple book shapes in the wine tones, two sizes | Library empty, no results |

## 9. Accessibility

- Text contrast at least 4.5 to 1. Borders, icons, and the focus ring at least 3 to 1.
- Status is never shown by color alone: a pill always has its text, an alert always has an icon.
- Visible focus ring on every clickable item. The page can be used with a keyboard only.
- Every field has a visible label. Errors sit under the field.
- Touch targets at least 44 px on mobile.
- Body text is never below 14 px, and inputs are 16 px so phones do not zoom.
- Code boxes: each box is labeled (`Digit 1 of 6`), and pasting fills all six.
- No horizontal scrolling from 320 px up.

## 10. Content to prepare

- Real sample data for mockups: about 12 books with different titles, long and short, with and without covers, in all three statuses. At least one long title and one long author.
- Sample notes with different lengths.
- Cover images for three or four books, all in a 2:3 ratio.
- A profile photo and a first-letter avatar.
- All messages (flash, errors, hints, empty states) from `docs/04-routes.md` and `docs/05-pages.md`, so the designs show real words and not placeholder text.

## 11. Process

1. Design mobile first, then tablet, then laptop and PC. Where a layout does not change, copy the frame and check it.
2. Design a story's screens before its code is written. Each story has a Design subtask in Jira, and the Figma links go in the story.
3. Build every screen from components, color variables, and text styles. No raw hex, no typed font sizes, no loose spacing.
4. Review each screen against the checklist in section 12. Then move the Design subtask to In Review, then Done.
5. After the code is built, compare the page with the Figma frame at 390, 768, and 1440 px. Small changes during coding are normal. If the design changes, update Figma and the docs.
6. Make a clickable prototype of the main flows on mobile: register, verify, and reach the library; add a book; add a note; edit the profile; change the password.

**Figma file** (free plan, 3 pages): `Cover`, `Design System`, `Screens`. The `Screens` page has the sections `Mobile`, `Tablet`, and `Desktop`. Frames are named `Mobile/Library`, `Tablet/Library`, `Desktop/Library`.

## 12. Review checklist for every screen

- [ ] It matches the page and the stories in `docs/05-pages.md` and `docs/02-user-stories.md`.
- [ ] Only components, color variables, and text styles are used.
- [ ] Spacing comes only from 4, 8, 12, 16, 24, 32, 48, 64, and everything is Auto layout.
- [ ] One H1. Headings use the Gentium styles, and the interface uses Inter.
- [ ] Copy follows the rule: only the first word is capitalized. Real messages are used.
- [ ] Every state is shown: default, error, empty, and loading where they apply.
- [ ] Contrast and touch targets are checked.
- [ ] The layout is checked on all three devices, and nothing overflows on a 320 px frame.
- [ ] The Figma link is in the Jira story.

## 13. Out of scope for the design

- Dark mode.
- Illustrations beyond the empty state art.
- Animation beyond hover, fade, and the spinner.
- Photography and stock images.
- Separate designs for landscape orientation and screens wider than 1440 px (the content stays centered with a maximum width of 1120).

## 14. References

Used for inspiration, never for copying.
- The StoryGraph and Goodreads: library and shelf layouts.
- Notion: calm forms and empty states.
- Mobbin, Dribbble, Behance: "book tracker", "library app", "reading app", "auth screens", "profile screen".
- Realtime Colors: to try the palette on a sample page. WebAIM: contrast checks.
- Lucide: icons. Google Fonts: Gentium Book Plus and Inter.

## 15. Deliverables

1. The design system in Figma and `docs/08-design-system.md`, matching each other.
2. 48 base frames (16 pages on 3 devices) and the state frames in section 6.1.
3. A clickable mobile prototype of the main flows.
4. The brand assets in section 8, exported as SVG into `frontend/public/`.
5. Sample content in the mockups (section 10).

## 16. Decisions to confirm

These are assumptions in the current design. Change any that you do not like.

| # | Decision | Where |
|---|---|---|
| 1 | The tablet navbar shows the full links. Only mobile has the hamburger menu | Pages 3.2 |
| 2 | On mobile the book card is horizontal (small cover at the left), because a vertical card would fill a whole phone screen | Pages 4.7 |
| 3 | The Want to read pill has a thin navy border, so it does not vanish on a white card | Design system 2.5 |
| 4 | Profile numbers: 2 columns on mobile, 3 on tablet, 6 in one row on laptop and PC | Pages 4.12 |
| 5 | Tablet uses the desktop heading sizes. Only mobile uses the Mobile heading styles | Pages 2 |
| 6 | Add Book stays in the navbar, and also appears on the library page header | Pages 3.2 |

## 17. Done when

- Every page in section 6 has a Mobile, Tablet, and Laptop and PC frame, plus the state frames.
- Every component in section 7 exists with all its states, and matches `docs/08-design-system.md`.
- Every screen passes the checklist in section 12.
- The prototype covers the main flows.
- The brand assets are made and exported.