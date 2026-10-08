# Design System: BookShelf v1

**Source:** docs/07-design-brief.md
**Figma:** file `BookShelf`, page `Design System`
**Date:** 7 Oct 2026

Token names in this file are also the CSS variable names (`primary` becomes `--primary`). Components use tokens only, never raw hex.

## 1. Principles

- Calm and bookish. Warm paper background, ink text, one brand color.
- One main action per screen.
- Status is never shown by color alone.
- Everything is reachable by keyboard.

## 2. Colors

Palette made with the 9 shade HSB arc method. 6 scales, 54 shades in Figma. Only the 28 tokens below are used.

### 2.1 Primary (ink blue): buttons, links, focus

| Token | Hex | Used for |
|---|---|---|
| `primary-light` | `#EBF7FF` | Selected items, hover background |
| `primary-focus` | `#3B6E8F` | Focus ring |
| `primary` | `#2D5F80` | Buttons, links, active nav, current page |
| `primary-dark` | `#1A4866` | Button hover and pressed |

### 2.2 Secondary (wine): decoration only

| Token | Hex | Used for |
|---|---|---|
| `secondary-light` | `#FFEBF4` | Soft decorative background |
| `secondary-focus` | `#A0607F` | Decorative icons, highlights |
| `secondary` | `#802D55` | Logo accent, dividers, empty state art |
| `secondary-dark` | `#661A3F` | Decorative text accents |

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

### 2.4 Status: light is background, middle is icon and border, dark is text

| Token | Hex | Used for |
|---|---|---|
| `success-light` | `#EBFFF1` | Success alert, Finished badge background |
| `success` | `#358550` | Success icon, border, 100% progress fill |
| `success-dark` | `#1A6633` | Success text |
| `warning-light` | `#FFF8EB` | Warning background |
| `warning` | `#A8803B` | Warning icon, border |
| `warning-dark` | `#664A1A` | Warning text |
| `error-light` | `#FFECEB` | Error background |
| `error` | `#A1453D` | Error icon, border, Delete button |
| `error-dark` | `#66201A` | Error text, form errors |
| `info-light` | `#EBFFFF` | Info alert, Reading badge background |
| `info` | `#3B9494` | Info icon, border |
| `info-dark` | `#1A6666` | Info text |

### 2.5 Status badges

| Badge | Background | Text |
|---|---|---|
| Want to read | `bg-muted` | `text` |
| Reading | `info-light` | `info-dark` |
| Finished | `success-light` | `success-dark` |

### 2.6 Color rules

- Text 4.5 to 1 contrast. Borders, icons, and focus ring 3 to 1. Checked in Stark.
- `text-soft` only on `bg` and `surface`. Not on `bg-muted` (too low).
- Text on colored backgrounds uses only the pairs above: `text-on-primary` on `primary`, status dark on status light.
- Status always has an icon or a text label as well as color.

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
| `Heading/H3` | Fraunces | 600 | 20 / 28 | 20 / 28 | Book title on cards, modal title |
| `Body/Regular` | Work Sans | 400 | 16 / 24 | 16 / 24 | Body text, inputs, notes, flash text |
| `Body/Medium` | Work Sans | 500 | 16 / 24 | 16 / 24 | Form labels, navbar links |
| `Body/Small` | Work Sans | 400 | 14 / 20 | 14 / 20 | Author, hints, form errors, footer |
| `Button` | Work Sans | 600 | 16 / 20 | 16 / 20 | Button text |
| `Label` | IBM Plex Mono | 500 | 12 / 16 | 12 / 16 | Badges, page numbers, progress numbers |

`Label` is uppercase with 4% letter spacing. Only Display and H1 change on mobile.

### 3.3 Type rules

- One H1 per page. Do not skip heading levels in HTML. Choose by meaning, not size.
- Fraunces is weight 600 only. No italics, no extra weights.
- `Label` is for short labels and numbers, never sentences.
- Body text never below 14 px. Inputs are 16 px so phones do not zoom.
- Long text (description, notes) stays 60 to 75 characters per line.

## 4. Spacing, shape, depth

| Item | Values |
|---|---|
| Spacing scale (px) | 4, 8, 12, 16, 24, 32, 48, 64 |
| Radius | `sm` 4 (inputs, badges), `md` 8 (cards, buttons, modal), `full` 999 (progress bar) |
| Border | 1 px `border` (cards, dividers), 1 px `border-strong` (inputs) |
| Shadow `sm` | `0 1px 2px rgba(38,36,31,0.08)`, cards |
| Shadow `md` | `0 4px 12px rgba(38,36,31,0.12)`, modal, card hover |
| Focus ring | 2 px `primary-focus`, 2 px offset, on every clickable item |

## 5. Layout

| | Desktop | Mobile |
|---|---|---|
| Figma frame | 1440 wide | 390 wide |
| Content width | max 1120, centered | full width |
| Grid | 12 columns, gutter 24 | 4 columns, gutter 16 |
| Side margin | auto | 16 |

- Breakpoints: 600 and 900 px.
- Library grid: 3 cards desktop, 2 tablet, 1 phone. 12 books per page.
- Forms (auth pages): one centered card, max width 420.
- Page padding top and bottom: 48 desktop, 24 mobile.

## 6. Icons

- SVG files in `frontend/public/icons/`. Default size 20 px, 24 px for large uses.
- One simple line style across all icons, same stroke width.
- Set: eyeOpen, eyeClosed, cross, menu, search, plus, edit, trash, check, warning, chevronLeft, chevronRight, google.
- An icon never stands alone: buttons with only an icon have an accessible label.

## 7. Components

Every component is designed once in Figma with its states, then reused in all screens.

| Component | Spec | States |
|---|---|---|
| **Button primary** | Height 44, padding 0 24, radius `md`. `primary` background, `text-on-primary` text, `Button` style. | default, hover (`primary-dark`), pressed (`primary-dark`), focus, disabled (`bg-muted`), loading (spinner, label kept) |
| **Button secondary** | Same size. `surface` background, 1 px `border-strong`, `text`. | default, hover (`bg-muted`), pressed, focus, disabled |
| **Button danger** | Same size. `error` background, `text-on-primary` text. Delete only. | default, hover (`error-dark`), focus, disabled |
| **Link** | `primary`, underline on hover. | default, hover, focus, visited same as default |
| **Text input, select, textarea** | Height 44 (textarea min 96), padding 0 12, radius `sm`, 1 px `border-strong`, `surface`, 16 px text. Label above (`Body/Medium`), hint or error below (`Body/Small`). | default, hover, focus (ring), filled, error (border `error`, message `error-dark`, icon), disabled (`bg-muted`) |
| **Password input** | Text input with a show or hide eye icon on the right. | same as input |
| **Checkbox** | 20 px square, radius `sm`, label right (Remember me). | unchecked, checked (`primary`), focus, disabled |
| **Code input (OTP)** | One centered input, 6 digits, `IBM Plex Mono` 24 px, wide letter spacing. | same as input |
| **Cover upload** | Dashed 1 px `border-strong` box with preview, file type and size hint, Replace button on edit. | empty, filled, error |
| **Search and filter bar** | Search input with search icon, status select, genre select, Clear link. No Apply button. | default, loading (list dims) |
| **Book card** | `surface`, 1 px `border`, radius `md`, shadow `sm`, padding 16. Cover on top (2:3), title `H3` (max 2 lines), author `Body/Small` `text-soft`, genre `Label`, status badge, progress bar with percent. | default, hover (shadow `md`), focus, no cover (placeholder) |
| **Status badge** | `Label` style, radius `sm`, padding 4 8. Colors from 2.5. Always has the text. | want to read, reading, finished |
| **Progress bar** | Height 8, radius `full`, track `bg-muted`, fill `primary`. Fill `success` at 100%. Percent in `Label` beside it. | 0%, partial, 100% |
| **Alert (flash)** | Status light background, 1 px status border, status icon, status dark text (`Body/Regular`), close icon. Max width 640, under the navbar. | success, info, warning, error |
| **Modal** | `surface`, radius `md`, shadow `md`, padding 24, max width 400. Overlay `text` at 50%. Title `H3`, text `Body/Regular`. Cancel (secondary) and Delete (danger), right aligned. | open. Esc and Cancel close it, focus stays inside |
| **Pagination** | Items 40 x 40, `Label` numbers, radius `sm`. Current page `primary` with `text-on-primary`. Others `primary` text, hover `primary-light`. Mobile: Prev, "PAGE 2 OF 5", Next. | default, hover, current, disabled (Prev on first page, Next on last) |
| **Navbar** | Height 64, `surface`, 1 px `border` at the bottom. Logo left, links `Body/Medium`, active link `primary` with underline. Mobile: hamburger menu. | visitor, logged in, menu open |
| **Note card** | `surface`, 1 px `border`, radius `md`, padding 16. Text `Body/Regular`, date and edited label `Body/Small` `text-soft`, page number `Label`, edit and delete icon buttons. | default, hover, no notes yet |
| **Empty state** | Decorative art in `secondary` tones, `H3`, one line of `Body/Regular`, one primary button. | no books, no search results, no notes |
| **Error page** | Status number in `Display`, message, Back to home button. | 404, 500 |
| **Footer** | `Body/Small`, `text-soft`, centered. | default |

## 8. Accessibility

- Touch targets at least 44 px on mobile.
- Every field has a visible label. Errors sit under the field and are tied to it.
- Visible focus ring on every clickable item.
- Nothing relies on color alone.
- Motion: only simple hover and fade. No other animation in v1.

## 9. Figma setup

- Color variables or styles named exactly as the tokens in section 2.
- Text styles named exactly as in 3.2.
- Effect styles: `shadow/sm`, `shadow/md`.
- Components named `Button/Primary`, `Input/Text`, `Card/Book`, `Badge/Status`, and so on. Variants for states.
- Pages: Cover, Moodboard, Design System, Wireframes, Desktop, Mobile, Prototype.

## 10. Done when

- Tokens, type styles, and effect styles exist in Figma and match this file.
- Every component in section 7 exists with all its states.
- Each component is used at least once in the mockups.