# Design Brief: BookShelf v1

**Source:** docs/00-client-request.md, docs/01-prd.md, docs/05-pages.md
**Date:** 7 Oct 2026

## 1. Purpose
BookShelf is a private reading library. The design should make a reader feel calm and organized, like a quiet library with a card catalog.

## 2. Users
- One reader, using the app alone, on a laptop or phone.
- Short visits: add a book, update the current page, write a note, find a book.

## 3. Key tasks to make easy
1. Add a book in under a minute.
2. Update reading progress in two clicks.
3. Find a book by search or filter.
4. Write a note while reading.
5. See the whole library and each status at a glance.

## 4. Personality
- **Keywords:** calm, warm, bookish, organized, trustworthy.
- **Not:** flashy, corporate, childish, crowded.

## 5. Visual direction
- Theme: library card catalog.
- Warm paper-like background, dark brown ink for text, one accent color.
- Cards look like index cards: thin borders, soft corners, light shadow.
- Serif font for headings, clean sans-serif for body, mono font for small labels and numbers.
- Plenty of white space. One main action per screen.

## 6. Starting choices (confirmed in the design system)
- Fonts to try: Fraunces (headings), Work Sans (body), IBM Plex Mono (labels and numbers).
- Colors: warm bone background, dark brown text, one accent (decide in design system).
- Status colors: want to read (grey), reading (blue), finished (green).

## 7. Screens to design

| # | Screen | Desktop | Mobile |
|---|---|---|---|
| 1 | Landing | yes | yes |
| 2 | Register | yes | yes |
| 3 | Verify email | yes | yes |
| 4 | Login | yes | yes |
| 5 | Forgot password | yes | yes |
| 6 | Reset password | yes | yes |
| 7 | Library | yes | yes |
| 8 | Add and edit book | yes | yes |
| 9 | Book details with notes | yes | yes |
| 10 | Edit note | yes | yes |
| 11 | Error page (404 and 500) | yes | yes |

**States to design**
- Library: empty, no search results, loading, one page only, many pages.
- Forms: default, focus, filled, error, disabled.
- Buttons: default, hover, pressed, disabled, loading.
- Alerts: success, error. Modal: delete confirmation.
- Book card: want to read, reading, finished, no cover.

## 8. Devices
- Desktop frame: 1440 wide.
- Mobile frame: 390 wide.
- Tablet follows the layout rules (2 column grid), no separate mockup.

## 9. Accessibility
- Text contrast at least 4.5 to 1.
- Status never shown by color alone. Always label plus color.
- Visible focus outline on every clickable item.
- Touch targets at least 44 px on mobile.
- Every form field has a visible label.

## 10. Out of scope for v1
- Dark mode.
- Illustrations and animation beyond simple hover and fade.
- Tablet mockups.

## 11. References (for inspiration, not copying)
- The StoryGraph, Goodreads: library and shelf layouts.
- Notion: calm forms and empty states.
- Dribbble, Behance, Mobbin: search "book tracker", "library app", "reading app".
- Coolors: color palettes. Google Fonts: font pairings.

## 12. Deliverables
1. Moodboard page in Figma.
2. Design system in Figma and `docs/08-design-system.md`.
3. Desktop and mobile mockups of all screens and states.
4. Clickable prototype of the main flows (register to library, add book, add note).
5. Handoff doc with exact values.

## 13. Done when
- Every screen in the list has a desktop and mobile design.
- Every page in `05-pages.md` is covered.
- Colors, fonts, and spacing are written in `08-design-system.md`.