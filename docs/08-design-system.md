# Design System: BookShelf v1

**Source:** docs/07-design-brief.md
**Figma:** file `BookShelf Design`, page `Design System`
**Date:** 9 Oct 2026

Token names in this file are also the CSS variable names (`primary` becomes `--primary`). Components use tokens only, never raw hex.

## 1. Principles

- Calm and bookish. Warm paper background, ink text, one brand color.
- One main action per screen.
- Status is never shown by color alone.
- Everything is reachable by keyboard.

## 2. Colors

Palette made with the 9 shade HSB arc method, 7 scales in Figma. Final tokens were chosen after WebAIM contrast tests. Only the 28 tokens below are used. Page is `bg` (warm off-white), cards are `surface` (white), and `primary-light` is only for small tints.

### 2.1 Primary: buttons, links, focus

| Token | Hex | Used for |
|---|---|---|
| `primary-light` | `#EBF7FF` | Selected items, hover background, active menu row |
| `primary-focus` | `#1A4866` | Focus ring |
| `primary` | `#0B334D` | Buttons, links, active nav, current page |
| `primary-dark` | `#00253D` | Button hover and pressed |

### 2.2 Secondary (wine): decoration only

| Token | Hex | Used for |
|---|---|---|
| `secondary-light` | `#FFEBF4` | Soft decorative background |
| `secondary-focus` | `#661A3F` | Decorative icons, highlights |
| `secondary` | `#4D0B2B` | Logo accent, dividers, empty state art |
| `secondary-dark` | `#3D001E` | Decorative accents |

Never use secondary for buttons, links, or anything that could look like an error.

### 2.3 Neutral (warm gray): most of the interface

| Token | Hex | Used for |
|---|---|---|
| `bg` | `#F4F2EE` | Page background |
| `bg-muted` | `#E8E5DF` | Hover and disabled backgrounds, progress track |
| `border` | `#D5D1C9` | Card borders, dividers |
| `border-strong` | `#8C8781` | Input borders, icons |
| `text-soft` | `#6E6A64` | Hints, secondary text |
| `text` | `#26241F` | Main text |
| `surface` | `#FFFFFF` | Cards, inputs, modal |
| `text-on-primary` | `#FFFFFF` | Text on primary and danger buttons |

### 2.4 Status: light is background, default is icon and border, dark is text

Light backgrounds are shade 200 of each scale, so alerts and badges stand out from the page.

| Token | Hex | Used for |
|---|---|---|
| `success-light` | `#C1E5CD` | Success alert, Finished badge background |
| `success` | `#1A6633` | Success icon, border, 100% progress fill |
| `success-dark` | `#0B4D21` | Success text |
| `warning-light` | `#E5D8C1` | Warning alert background |
| `warning` | `#664A1A` | Warning icon, border |
| `warning-dark` | `#4D350B` | Warning text |
| `error-light` | `#E5C4C1` | Error alert background |
| `error` | `#66201A` | Error icon, border, Delete button |
| `error-dark` | `#4D110B` | Error text, form errors |
| `info-light` | `#C1E5E5` | Info alert, Reading badge background |
| `info` | `#1A6666` | Info icon, border |
| `info-dark` | `#0B4D4D` | Info text |

### 2.5 Status badges

| Badge | Background | Text |
|---|---|---|
| Want to read | `bg-muted` | `text` |
| Reading | `info-light` | `info-dark` |
| Finished | `success-light` | `success-dark` |

### 2.6 Contrast (WebAIM, WCAG AA)

Text needs 4.5 to 1. Borders, icons, and the focus ring need 3 to 1. `~` means estimated, to confirm in WebAIM.

| Pair | Ratio | Needs |
|---|---|---|
| `text` on `bg` | 13.86 | 4.5 |
| `text` on `surface` | 15.50 | 4.5 |
| `text` on `bg-muted` | 12.33 | 4.5 |
| `text` on `primary-light` | 14.23 | 4.5 |
| `text-soft` on `bg` | 4.80 | 4.5 |
| `text-soft` on `surface` | 5.37 | 4.5 |
| `text-on-primary` on `primary` | 13.16 | 4.5 |
| `text-on-primary` on `primary-dark` | 15.76 | 4.5 |
| `text-on-primary` on `error` | ~11.8 | 4.5 |
| `text-on-primary` on `error-dark` | ~15 | 4.5 |
| `primary` on `bg` | 11.77 | 4.5 |
| `primary` on `surface` | 13.16 | 4.5 |
| `primary` on `primary-light` | 12.08 | 4.5 |
| `success-dark` on `success-light` | ~7.3 | 4.5 |
| `info-dark` on `info-light` | ~7.2 | 4.5 |
| `warning-dark` on `warning-light` | ~8.2 | 4.5 |
| `error-dark` on `error-light` | ~9.3 | 4.5 |
| `error-dark` on `bg` | ~13.4 | 4.5 |
| `border-strong` on `surface` | 3.56 | 3 |
| `border-strong` on `bg` | 3.18 | 3 |
| `primary-focus` on `bg` | 8.68 | 3 |
| `primary-focus` on `surface` | 9.70 | 3 |
| `success` on `success-light` | ~5.1 | 3 |
| `info` on `info-light` | ~5.0 | 3 |
| `warning` on `warning-light` | ~5.8 | 3 |
| `error` on `error-light` | ~7.3 | 3 |
| `error` on `surface` | ~11.8 | 3 |

### 2.7 Color rules

- Components use tokens only, never raw hex.
- `text-soft` only on `bg` and `surface`. Not on `bg-muted`.
- Text on a status light background uses only that status's `-dark` token.
- `border-strong` and the status default colors (`success`, `warning`, `error`, `info`) are for borders and icons only, never text. `error-dark` is the text color for form errors.
- Text on colored backgrounds uses only the pairs in 2.6.
- Status always has an icon or a text label as well as color.
- `secondary` is decoration only.

## 3. Typography

### 3.1 Fonts (free, Google Fonts)

| Role | Font | Weights | Fallback |
|---|---|---|---|
| Heading | Fraunces | 600 | Georgia, serif |
| Body | Work Sans | 400, 500, 600 | system-ui, sans-serif |
| Label | IBM Plex Mono | 500 | ui-monospace, monospace |

### 3.2 Type scale

| Style (Figma name) | Font | Weight | Desktop size / line | Mobile size / line | Used for |
|---|---|---|---|---|---|
| `Heading/Display` | Fraunces | 600 | 40 / 48 | 32 / 40 | Landing hero only |
| `Heading/H1` | Fraunces | 600 | 32 / 40 | 28 / 36 | Page titles |
| `Heading/H2` | Fraunces | 600 | 24 / 32 | 24 / 32 | Section headings (Notes, Description) |
| `Heading/H3` | Fraunces | 600 | 20 / 28 | 20 / 28 | Book title on cards, modal title, wordmark |
| `Body/Regular` | Work Sans | 400 | 16 / 24 | 16 / 24 | Body text, inputs, notes, flash text |
| `Body/Medium` | Work Sans | 500 | 16 / 24 | 16 / 24 | Form labels, navbar links |
| `Body/Small` | Work Sans | 400 | 14 / 20 | 14 / 20 | Author, hints, form errors, footer |
| `Button` | Work Sans | 600 | 16 / 20 | 16 / 20 | Button text |
| `Label` | IBM Plex Mono | 500 | 12 / 16 | 12 / 16 | Badges, page numbers, progress numbers |

In Figma the mobile heading sizes are the styles `Heading/Display Mobile` and `Heading/H1 Mobile`. `Label` is uppercase with 4% letter spacing.

### 3.3 Type rules

- One H1 per page. Do not skip heading levels in HTML. Choose by meaning, not size.
- Fraunces is weight 600 only. No italics, no extra weights.
- `Label` is for short labels and numbers, never sentences.
- Body text never below 14 px. Inputs are 16 px so phones do not zoom.
- Long text (description, notes) stays 60 to 75 characters per line.

## 4. Spacing, shape, depth

| Item | Values |
|---|---|
| Spacing scale (px) | 4, 8, 12, 16, 24, 32, 48, 64. Padding, gaps, and margins use only these |
| Radius | `sm` 4 (inputs, badges), `md` 8 (cards, buttons, modal), `full` 999 (progress bar) |
| Border | 1 px `border` (cards, dividers), 1 px `border-strong` (inputs) |
| Shadow `sm` | `0 1px 2px rgba(38,36,31,0.08)`, cards |
| Shadow `md` | `0 4px 12px rgba(38,36,31,0.12)`, modal, card hover |
| Focus ring | 2 px `primary-focus` ring with a 2 px gap in `bg`, on every clickable item |

## 5. Layout

| | Desktop | Mobile |
|---|---|---|
| Figma frame | 1440 wide | 390 wide |
| Content width | max 1120, centered | full width |
| Grid | 12 columns, gutter 24, margin 160 | 4 columns, gutter 16, margin 16 |

- Breakpoints: 600 and 900 px.
- Library grid: 3 cards desktop, 2 tablet, 1 phone. 12 books per page.
- Auth pages: one centered card, 420 wide.
- Page padding top and bottom: 48 desktop, 24 mobile.
- Mobile first. Each screen is designed at 390 first, then 1440. CSS base styles are for phones, with `min-width` media queries at 600 and 900 px.

## 6. Icons

- Source: Lucide (free). One line style, stroke width 2, 24 px frame, stroke color `text` unless a component sets it. The Google logo is the official one, in its original colors.
- Set (16): eyeOpen, eyeClosed, cross, menu, search, plus, edit, trash, check, info, warning, error, chevronLeft, chevronRight, google, spinner.
- Alert icons take their status color inside the alert.
- An icon never stands alone: a button with only an icon has an accessible label.
- SVG files go in `frontend/public/icons/` at build time.

## 7. Components

Each component is designed once in Figma with all its states, then reused in every screen.

| Component | Spec | States |
|---|---|---|
| **Button, Primary** | Height 44, padding 24 sides, radius `md`. `primary` fill, `text-on-primary` text, style `Button`. | default, hover (`primary-dark`), pressed (`primary-dark`), focus, disabled (`bg-muted` fill, `text-soft` text), loading (spinner, label `Saving...`) |
| **Button, Outline** | Same size. `surface` fill, 1 px `border-strong`, `text` text. | default, hover (`bg-muted`), pressed, focus, disabled, loading |
| **Button, Danger** | Same size. `error` fill, `text-on-primary` text. Delete only. | default, hover (`error-dark`), pressed, focus, disabled, loading |
| **Link** | `primary`, underline on hover. | default, hover, focus |
| **Input, Text** | Label above (`Body/Medium`), field height 44, padding 12 sides, radius `sm`, 1 px `border-strong`, `surface`, 16 px text, hint or error below (`Body/Small`). Properties: Label, Placeholder, Message, ShowMessage. | default, hover, focus, filled, error (border `error`, icon, message in `error-dark`), disabled |
| **Input, Password** | Input, Text plus a show or hide eye icon on the right. | same as Input, Text |
| **Input, Select** | Input, Text plus a chevron on the right. | same as Input, Text |
| **Input, Textarea** | Input, Text with min height 96. | same as Input, Text |
| **Input, OTP** | One field, 6 digits, `IBM Plex Mono` 24 px, centered, wide letter spacing. | same as Input, Text |
| **Checkbox** | 20 px square, radius `sm`, label right (`Body/Regular`). Checked: `primary` fill with a white check. | unchecked, checked, focus, disabled |
| **Divider with text** | 1 px `border` line, `or` in `Body/Small` `text-soft`, line again. | default |
| **Auth card** | 420 wide, `surface`, 1 px `border`, radius `md`, shadow `sm`, padding 32, gap 24. Used by login, register, verify, forgot, and reset. | default |
| **Cover upload** | Dashed 1 px `border-strong` box with preview, file type and size hint, Replace button on edit. | empty, filled, error |
| **Search and filter bar** | Search input with search icon, status select, genre select, Clear link. No Apply button. | default, loading (list dims) |
| **Book card** | `surface`, 1 px `border`, radius `md`, shadow `sm`, padding 16. Cover on top (2:3), title `H3` (max 2 lines), author `Body/Small` `text-soft`, genre `Label`, status badge, progress bar with percent. | default, hover (shadow `md`), focus, no cover (placeholder) |
| **Status badge** | `Label` style, radius `sm`, padding 4 and 8. Colors from 2.5. Always has the text. | want to read, reading, finished |
| **Progress bar** | Height 8, radius `full`, track `bg-muted`, fill `primary`. Fill `success` at 100%. Percent in `Label` beside it. | 0%, partial, 100% |
| **Alert (flash)** | Status light background, 1 px status border, status icon, status dark text (`Body/Regular`), close icon. Max width 640, under the navbar. | success, info, warning, error |
| **Modal** | `surface`, radius `md`, shadow `md`, padding 24, max width 400. Overlay `text` at 50%. Title `H3`, text `Body/Regular`. Cancel (Outline) and Delete (Danger), right aligned. | open. Esc and Cancel close it, focus stays inside |
| **Pagination** | Items 40 x 40, `Label` numbers, radius `sm`. Current page `primary` with `text-on-primary`. Others `primary` text, hover `primary-light`. Mobile: Prev, `PAGE 2 OF 5`, Next. | default, hover, current, disabled (Prev on first page, Next on last) |
| **Navbar, Visitor** | Height 64, `surface`, 1 px `border` at the bottom, side padding 160 (desktop). Left: wordmark `BookShelf` in `H3` with a small bar in `secondary` before it. Right: `Login` link and Primary button `Register`. Mobile: hamburger menu. | default, menu open |
| **Navbar, User** | Same frame. Links `My Library` and `Add Book`, username, `Logout` button. Active link `primary` with underline. | default, menu open |
| **Note card** | `surface`, 1 px `border`, radius `md`, padding 16. Text `Body/Regular`, date and edited label `Body/Small` `text-soft`, page number `Label`, edit and delete icon buttons. | default, hover |
| **Empty state** | Decorative art in `secondary` tones, `H3`, one line of `Body/Regular`, one Primary button. | no books, no search results, no notes |
| **Error page** | Status number in `Display`, message, Back to home Primary button. | 404, 500 |
| **Footer** | `Body/Small`, `text-soft`, centered, padding 24. | default |

## 8. Accessibility

- Touch targets at least 44 px on mobile.
- Every field has a visible label. Errors sit under the field and are tied to it.
- Visible focus ring on every clickable item.
- Nothing relies on color alone.
- Motion: only simple hover and fade. No other animation in v1.

## 9. Figma setup

- Free plan allows 3 pages per file: `Cover`, `Design System`, `Screens`. `Design System` has sections Colors, Typography, Spacing and Shadows, Icons, Components. `Screens` has sections Desktop and Mobile. The prototype is built on the `Screens` page.
- Color variables are in a collection `Colors`, grouped by family: `primary`, `secondary`, `neutral`, `success`, `warning`, `error`, `info`. Figma `primary/dark` is the token `primary-dark` and the CSS variable `--primary-dark`. The base shade of a family is `default` (`primary/default` is `--primary`). Neutral names drop the group in CSS (`neutral/bg` is `--bg`).
- Text styles are named exactly as in 3.2.
- Shadows and spacing follow section 4. Components use Auto layout with spacing values from the scale only.
- Components are named `Button`, `Input/Text`, `Card/Book`, `Badge/Status`, and so on, with variants for states.
- Screen frames are named `Desktop/Login`, `Mobile/Login`, and so on.

## 10. Done when

- Variables, text styles, and icons exist in Figma and match this file.
- Every component in section 7 exists with all its states.
- Each component is used at least once in the mockups.