# Design System: BookShelf v1

**Source:** docs/07-design-brief.md
**Figma:** file `BookShelf Design`
**Date:** 14 Oct 2026

Token names in this file are also the CSS variable names (`primary` becomes `--primary`). Components use tokens only, never raw hex.

## 1. Principles

- Calm and bookish. Warm paper background, white cards, ink text.
- Serif for what you read (titles, descriptions, notes). Sans-serif for what you use (buttons, forms, navigation, labels).
- One main action per screen.
- Status is never shown by color alone.
- Everything is reachable by keyboard.
- Mobile first. Each screen is designed at 390 px, then 1440 px.

## 2. Colors

Palette made with the 9 shade HSB arc method, 7 scales in Figma. Final tokens were chosen after WebAIM contrast tests. Only the 28 tokens below are used. The page is `bg` (warm off-white), cards are `surface` (white), and `primary-light` is only for small tints.

### 2.1 Primary: buttons, links, focus

| Token | Hex | Used for |
|---|---|---|
| `primary-light` | `#EBF7FF` | Selected items, hover background, active menu row, Want to read pill |
| `primary-focus` | `#1A4866` | Focus ring, Want to read pill border |
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
| `bg-muted` | `#E8E5DF` | Hover and disabled backgrounds, progress track, cover panel |
| `border` | `#D5D1C9` | Card borders, dividers |
| `border-strong` | `#8C8781` | Input borders, input icons |
| `text-soft` | `#6E6A64` | Hints, secondary text |
| `text` | `#26241F` | Main text |
| `surface` | `#FFFFFF` | Cards, inputs, modal |
| `text-on-primary` | `#FFFFFF` | Text on primary and danger buttons |

### 2.4 Status: light is background, default is icon and border, dark is text

Light backgrounds are shade 200 of each scale, so alerts and pills stand out from the page.

| Token | Hex | Used for |
|---|---|---|
| `success-light` | `#C1E5CD` | Success alert, Finished pill background |
| `success` | `#1A6633` | Success icon, border, 100% progress fill |
| `success-dark` | `#0B4D21` | Success text |
| `warning-light` | `#E5D8C1` | Warning alert background |
| `warning` | `#664A1A` | Warning icon, border |
| `warning-dark` | `#4D350B` | Warning text |
| `error-light` | `#E5C4C1` | Error alert background |
| `error` | `#66201A` | Error icon, border, Delete button |
| `error-dark` | `#4D110B` | Error text, form errors |
| `info-light` | `#C1E5E5` | Info alert, Reading pill background |
| `info` | `#1A6666` | Info icon, border |
| `info-dark` | `#0B4D4D` | Info text |

### 2.5 Status pills

Fully rounded. Same 1 px border on all three so they keep the same size.

| Pill | Fill | Border | Text |
|---|---|---|---|
| Want to read | `primary-light` | `primary-focus` | `primary` |
| Reading | `info-light` | `info-light` | `info-dark` |
| Finished | `success-light` | `success-light` | `success-dark` |

### 2.6 Contrast (WebAIM, WCAG AA)

Text needs 4.5 to 1. Borders, icons, and the focus ring need 3 to 1. A `~` means estimated, to confirm in WebAIM.

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
| Headings, wordmark, reading text | Gentium Book Plus | 400, 700 | Georgia, serif |
| Interface | Inter | 400, 500, 600 | system-ui, sans-serif |

### 3.2 Type scale

| Style (Figma name) | Font | Weight | Desktop size / line | Mobile size / line | Used for |
|---|---|---|---|---|---|
| `Heading/Display` | Gentium Book Plus | 700 | 44 / 52 | 36 / 44 | Landing hero only |
| `Heading/H1` | Gentium Book Plus | 700 | 32 / 40 | 28 / 36 | Page titles |
| `Heading/H2` | Gentium Book Plus | 700 | 24 / 32 | 24 / 32 | Section headings |
| `Heading/H3` | Gentium Book Plus | 700 | 20 / 28 | 20 / 28 | Book title on cards, modal title, wordmark |
| `Reading/Regular` | Gentium Book Plus | 400 | 17 / 28 | 17 / 28 | Book description, notes |
| `Body/Regular` | Inter | 400 | 16 / 24 | 16 / 24 | Inputs, alert text, UI text |
| `Body/Medium` | Inter | 500 | 16 / 24 | 16 / 24 | Form labels, nav links |
| `Body/Small` | Inter | 400 | 14 / 20 | 14 / 20 | Author, hints, form errors, footer |
| `Button` | Inter | 600 | 16 / 20 | 16 / 20 | Button text |
| `Label` | Inter | 500 | 12 / 16 | 12 / 16 | Pills, genre, page numbers, progress numbers |
| `Code` | Inter | 600 | 24 / 32 | 24 / 32 | OTP digits |

In Figma the mobile sizes are the styles `Heading/Display Mobile` and `Heading/H1 Mobile`. `Label` is uppercase with 4% letter spacing. `Code` has 0% letter spacing, because each digit sits in its own box.

### 3.3 Type rules

- One H1 per page. Do not skip heading levels in HTML. Choose by meaning, not size.
- Gentium Book Plus has only weights 400 and 700. Headings are always 700.
- `Reading/Regular` is for long text only (descriptions, notes). Interface text is always Inter.
- `Label` is for short labels and numbers, never sentences.
- Interface text is never below 14 px. Inputs are 16 px so phones do not zoom.
- Long text stays 60 to 75 characters per line.
- Copy style: only the first word of a button, link, or label is capitalized (`Forgot password?`, `Save book`).

## 4. Spacing, shape, depth

| Item | Values |
|---|---|
| Spacing scale (px) | 4, 8, 12, 16, 24, 32, 48, 64. Padding, gaps, and margins use only these |
| Radius | `sm` 4 (inputs, OTP boxes, checkbox), `md` 8 (cards, buttons, alerts, modal), `full` 999 (status pills, progress bar) |
| Border | 1 px `border` (cards, dividers), 1 px `border-strong` (inputs) |
| Shadow `sm` | `0 1px 2px rgba(38,36,31,0.08)`, cards |
| Shadow `md` | `0 4px 12px rgba(38,36,31,0.12)`, modal, card hover |
| Focus ring | 2 px gap in `bg`, then 2 px ring in `primary-focus`, on every clickable item |

## 5. Layout

| | Desktop | Mobile |
|---|---|---|
| Figma frame | 1440 wide | 390 wide |
| Content width | max 1120, centered | full width |
| Grid | 12 columns, gutter 24, margin 160 | 4 columns, gutter 16, margin 16 |

- Mobile first. CSS base styles are for phones, with `min-width` media queries at 600 and 900 px.
- Library grid: 3 cards desktop, 2 tablet, 1 phone. 12 books per page.
- Auth pages: one centered card, 420 wide on desktop, full width with 16 margin on mobile.
- Page padding top and bottom: 48 desktop, 24 mobile.

## 6. Icons

### 6.1 Source and style

- Source: Lucide (free, open source). The Google logo is the official one, in its original colors.
- One line style for all icons: stroke width 2, rounded ends and joins, no fill.
- Every icon is a 24 x 24 frame, named `Icon/name` in Figma.
- Default stroke color is `text`. The component that uses an icon sets the color (see 6.3).

### 6.2 Icon set (20)

| Figma component | Lucide name | Used for |
|---|---|---|
| `Icon/eyeOpen` | eye | Show password |
| `Icon/eyeClosed` | eye-off | Hide password |
| `Icon/cross` | x | Close alert and modal |
| `Icon/menu` | menu | Mobile hamburger |
| `Icon/search` | search | Search input |
| `Icon/plus` | plus | Add book |
| `Icon/edit` | pencil | Edit book and note |
| `Icon/trash` | trash-2 | Delete book and note |
| `Icon/check` | check | Checkbox |
| `Icon/verified` | circle-check | Success alert |
| `Icon/info` | info | Info alert |
| `Icon/warning` | triangle-alert | Warning alert |
| `Icon/error` | circle-alert | Error alert, input error |
| `Icon/chevronLeft` | chevron-left | Pagination Prev |
| `Icon/chevronRight` | chevron-right | Pagination Next |
| `Icon/chevronDown` | chevron-down | Select |
| `Icon/book` | book | No-cover placeholder |
| `Icon/upload` | upload | Cover upload |
| `Icon/spinner` | loader-circle | Loading |
| `Icon/google` | official logo | Continue with Google |

### 6.3 Sizes and colors

| Where | Size | Stroke color |
|---|---|---|
| Input icons (search, eye, chevron) | 20 | `border-strong` |
| Input error icon | 16 | `error` |
| Buttons (left icon, Google) | 20 | same as the button text |
| Icon buttons (edit, delete, close) | 20 | edit and close `text-soft`, delete `error` |
| Alerts | 20 | status default (`success`, `info`, `warning`, `error`) |
| Checkbox check | 16 | `text-on-primary` |
| Pagination chevrons | 16 | `text` |
| Navbar menu | 24 | `text` |
| No-cover placeholder | 32 | `text-soft` |
| Cover upload | 24 | `text-soft` |
| Spinner | 20 | same as the text next to it |

Sizes are 16, 20, 24, or 32 only. Colors come from tokens, never raw hex.

### 6.4 Rules

- An icon never carries meaning alone. A button with only an icon has an accessible label (`aria-label` in HTML).
- Decorative icons next to text are hidden from screen readers.
- Status icons (`verified`, `info`, `warning`, `error`) always come with text.
- The spinner rotates, one turn per second. This is the only animation in v1 besides hover and fade.
- Do not mix in icons from other sets.

### 6.5 Build and delivery

- At build time, export each icon as SVG into `frontend/public/icons/` (`eyeOpen.svg`, `trash.svg`, and so on).
- Use `stroke="currentColor"` and no fixed width and height in the SVG, so CSS controls color and size.
- The Google logo SVG keeps its fixed colors.

## 7. Components

### 7.1 Global rules

- Auto layout on every component. Width is Fill unless a spec says otherwise.
- Colors come from variables, text from text styles, spacing from the scale in section 4.
- Names are `Group/Name`. Variant properties are in Title Case: `State`, `Style`, `Type`, `Device`.
- A `Device` property (Mobile, Desktop) exists only where the layout really differs.
- Focus ring: two drop shadows, X 0, Y 0, Blur 0. First spread 2 in `bg`, second spread 4 in `primary-focus`.
- Touch targets are at least 44 px on mobile.

### 7.2 Actions

| Component | Spec | Variants |
|---|---|---|
| **Button** | Height 44, padding 12 top and bottom, 24 sides, gap 8, radius `md`. Text `Button`. Optional left icon (20). Width Hug or Fill. Primary: `primary` fill, `text-on-primary` text. Outline: `surface` fill, 1 px `border-strong`, `text` text. Danger: `error` fill, `text-on-primary` text, for Delete only. Google button is Outline with `Icon/google`. | Style: Primary, Outline, Danger. State: Default, Hover (Primary `primary-dark`, Danger `error-dark`, Outline `bg-muted`), Pressed (same as Hover), Focus (ring), Disabled (`bg-muted` fill, `text-soft` text, 1 px `border`), Loading (spinner, label `Saving...`) |
| **Button/Icon** | 40 x 40, radius `md`, icon 20. Icon `text-soft`, Danger icon `error`. Hover fill `bg-muted` (Danger `error-light`). Disabled icon `border`. On mobile the tap area is 44 x 44. | Type: Default, Danger. State: Default, Hover, Focus, Disabled |
| **Link** | `Body/Medium` or `Body/Small`. Inline `primary`. Nav `text`, hover `primary`. Nav Active `primary` with underline (offset 4). | Type: Inline, Nav, Nav Active. Size: Medium, Small. State: Default, Hover (underline), Focus (ring) |

### 7.3 Forms

| Component | Spec | Variants |
|---|---|---|
| **Input/Text** | Label above (`Body/Medium`, `text`). Field height 44, padding 12 sides, gap 8, radius `sm`, `surface` fill, 1 px `border-strong`, value `Body/Regular`. Message below (`Body/Small`): a hint is `text-soft`. An error is `error-dark` with `Icon/error` (16) and replaces the hint. Optional left icon (20). Properties: Label, Placeholder, Message, ShowLabel, ShowMessage, IconLeft. | State: Default, Hover (border `text-soft`), Focus (border `primary` plus ring), Filled, Error (border `error`), Disabled (`bg-muted` fill, `border` stroke) |
| **Input/Password** | Input/Text plus an eye icon (20, `border-strong`) at the right. Instance swap between `eyeOpen` and `eyeClosed`. Filled shows dots. | Same as Input/Text |
| **Input/Select** | Input/Text plus `Icon/chevronDown` at the right. The open list is the browser's native select. | Same as Input/Text |
| **Input/Textarea** | Input/Text with min height 96, padding 12 on all sides, text aligned top. Optional counter `0 / 2000` (`Body/Small`, `text-soft`) at the right of the message row. | Same as Input/Text |
| **Input/OTP** | Label, a row of 6 `OTP/Box` with gap 8, width Fill, then the message row. Boxes share the width (min 40). Digits only. | State: Default, Focus, Typing, Filled, Error, Disabled |
| **OTP/Box** | Height 56, radius `sm`, `surface` fill, 1 px `border-strong`, one digit in `Code` style, centered. Caret in `primary` when focused. | State: Empty, Focus (border `primary` plus ring), Filled, Error (border `error`), Disabled |
| **Checkbox** | Box 20 x 20, radius `sm`, `surface` fill, 1 px `border-strong`. Label `Body/Regular` at the right, gap 8. Checked: `primary` fill with `Icon/check`. | State: Unchecked, Checked, Focus (ring on the box), Disabled |
| **Divider/Text** | Two 1 px `border` lines with `or` (`Body/Small`, `text-soft`) between, gap 12. | Default |
| **Cover Upload** | Label `Cover`. Dashed 1 px `border-strong` box, radius `md`, padding 24, `surface` fill. Empty: `Icon/upload`, `Choose a cover image`, hint `JPG, PNG or WEBP, max 2 MB`. Filled: preview 2:3 (desktop 160 x 240, mobile 120 x 180, radius `sm`) and an Outline `Replace` button. | State: Empty, Filled, Error. Device |
| **Form/Actions** | Cancel (Outline) and Save (Primary). Desktop: horizontal, right aligned, gap 12. Mobile: vertical, gap 12, Fill, Primary on top. | Device |

### 7.4 Feedback

| Component | Spec | Variants |
|---|---|---|
| **Alert** | Radius `md`, 1 px status border, status light fill, padding 12 top and bottom, 16 sides, gap 12, top aligned. Status icon (20, status default color). Text `Body/Regular` in status dark. Close `Icon/cross`. Width Fill, max 640. | Type: Success (`Icon/verified`), Info (`Icon/info`), Warning (`Icon/warning`), Error (`Icon/error`). Properties: Message, ShowClose |
| **Modal/Confirm** | Panel: `surface`, radius `md`, shadow `md`, padding 24, gap 16, max width 400. Title `Heading/H3`, message `Body/Regular`. Overlay is `text` at 50%, added per screen. Desktop: Cancel (Outline) and Delete (Danger) right aligned. Mobile: stacked, Fill, Delete on top. | Device |
| **Badge/Status** | Status pill: height 24, padding 4 top and bottom, 12 sides, radius `full`, 1 px border, `Label` text. Colors from 2.5. | Type: Want to read, Reading, Finished |
| **Progress/Bar** | Track height 8, radius `full`, `bg-muted`. Fill `primary`, or `success` at 100%. Percent in `Label`, fixed width 40, right aligned, `text-soft` (`success-dark` when complete). Width Fill. | State: Empty, Partial, Complete |

### 7.5 Navigation and layout

| Component | Spec | Variants |
|---|---|---|
| **Navbar/Visitor** | `surface` fill, 1 px `border` at the bottom. Wordmark: a 4 x 20 bar in `secondary` (radius 2), gap 8, `BookShelf` in `Heading/H3`. Desktop: 1440 x 64, side padding 160, right side Link `Login` (Nav) and Primary button `Register`, gap 24. Mobile: 390 x 56, side padding 16, right side `Button/Icon` with `Icon/menu`. | Device |
| **Navbar/User** | Same frame. Desktop: Links `My Library` and `Add Book` (the active one is Nav Active), username (`Body/Small`, `text-soft`), Outline `Logout`. Mobile: same as the visitor mobile navbar. | Device. Active: My Library, Add Book, None |
| **Menu/Mobile** | Opens under the navbar. Width Fill, `surface`, bottom 1 px `border`, rows 48 high, `Body/Medium`. Active row: `primary-light` fill, `primary` text. Visitor: Login, Register. User: My Library, Add Book, username, Outline `Logout` (Fill). Mobile only. | Type: Visitor, User |
| **Footer** | `BookShelf 2026`, `Body/Small`, `text-soft`, centered, padding 24. | Default |
| **Page Header** | Title `Heading/H1`, optional subtitle (`Body/Regular`, `text-soft`), optional Primary button. Desktop: one row, space between. Mobile: stacked, title `Heading/H1 Mobile`, button Fill. | Device |
| **Pagination** | Items 40 x 40, radius `sm`, `Label` text. Current: `primary` fill, `text-on-primary`. Others: `primary` text, hover `primary-light`. Prev and Next: height 40, 1 px `border-strong`, `Body/Medium`, chevron 16. Disabled: `bg-muted` fill, `border` stroke, `text-soft`. Desktop: Prev, numbers with `...` gaps, Next. Mobile: Prev, `PAGE 2 OF 5`, Next. | Device. Position: First, Middle, Last |

### 7.6 Library and book content

| Component | Spec | Variants |
|---|---|---|
| **Toolbar/Library** | Search `Input/Text` (left icon search, placeholder `Title or author`), status and genre `Input/Select`, Link `Clear` (Inline, Small). Desktop: one row, gap 16, selects 200 wide. Mobile: search, then two selects side by side, then Clear. No Apply button. Loading: spinner replaces the search icon. | Device. State: Default, Loading |
| **Card/Book** | `surface`, 1 px `border`, radius `md`, shadow `sm`, padding 16. Details (gap 8): title `Heading/H3` (max 2 lines), author `Body/Small` (`text-soft`), genre `Label` (`text-soft`), Badge/Status, Progress/Bar. Desktop: vertical card, cover panel (`bg-muted`, height 264, padding 12) with the cover 160 x 240 (radius `sm`). Mobile: horizontal card, cover 88 x 132 at the left. No cover: `Icon/book` (32, `text-soft`) on `bg-muted`. | Device. State: Default, Hover (shadow `md`), Focus. Cover: Image, None |
| **Book/Header** | Desktop: cover 240 x 360 at the left. At the right: title `Heading/H1`, author `Body/Regular` (`text-soft`), genre `Label` and pill, `Pages 128 / 300`, Progress/Bar (max 360), dates (`Body/Small`, `text-soft`), Outline `Edit` and Danger `Delete`. Mobile: centered, cover 160 x 240, title `Heading/H1 Mobile`, buttons side by side and Fill. | Device |
| **Card/Note** | `surface`, 1 px `border`, radius `md`, padding 16, gap 16. Text `Reading/Regular`. Meta row: `PAGE 42` (`Label`, `text-soft`), date and `edited` (`Body/Small`, `text-soft`). Right: `Button/Icon` edit and Danger delete. | Default |
| **Form/Note** | `Input/Textarea` (Label `Add a note`, counter on), `Input/Text` for the page (width 120), Primary `Add note`. Desktop: the page and the button on one row. Mobile: stacked, button Fill. | Device |

### 7.7 Page level

| Component | Spec | Variants |
|---|---|---|
| **Card/Auth** | Container for login, register, verify, forgot, and reset. `surface`, 1 px `border`, radius `md`. Desktop: width 420, padding 32, gap 24, shadow `sm`. Mobile: width Fill, padding 24, gap 24, no shadow. | Device |
| **Card/Feature** | Landing feature. Icon in a 40 px circle (`primary-light`, icon `primary`), title `Heading/H3`, text `Body/Regular` (`text-soft`). Padding 24, gap 12, `surface`, 1 px `border`, radius `md`. | Default |
| **Empty State** | Art in `secondary` tones (desktop 160, mobile 120), title `Heading/H3`, one line `Body/Regular` (`text-soft`), optional button. No books: Primary `Add a book`. No results: Outline `Clear filters`. No notes: no button. | Type: No books, No results, No notes. Device |
| **Error State** | Status number in `Heading/Display`, message in `Heading/H2`, Primary `Back to home`. 404: `This page was not found.` 500: `Something went wrong. Try again later.` | Type: 404, 500. Device |

## 8. Accessibility

- Touch targets at least 44 px on mobile.
- Every field has a visible label. Errors sit under the field and are tied to it.
- Visible focus ring on every clickable item.
- Nothing relies on color alone.
- OTP boxes: each has a label (`Digit 1 of 6`), and pasting a code fills all six.
- Motion: only the spinner, hover, and fade. No other animation in v1.

## 9. Figma setup

- Free plan allows 3 pages per file: `Cover`, `Design System`, `Screens`. `Design System` has sections Colors, Typography, Icons, Components. `Screens` has sections Desktop and Mobile. The prototype is built on the `Screens` page.
- Color variables are in a collection `Colors`, grouped by family: `primary`, `secondary`, `neutral`, `success`, `warning`, `error`, `info`. Figma `primary/dark` is the token `primary-dark` and the CSS variable `--primary-dark`. The base shade of a family is `default` (`primary/default` is `--primary`). Neutral names drop the group in CSS (`neutral/bg` is `--bg`).
- Text styles are named exactly as in 3.2.
- Components use Auto layout with spacing values from the scale only.
- Screen frames are named `Mobile/Login`, `Desktop/Login`, and so on.

## 10. Done when

- Variables, text styles, and icons exist in Figma and match this file.
- Every component in section 7 exists with all its variants.
- Each component is used at least once in the mockups.