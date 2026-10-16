# Design System: BookShelf v1

**Source:** docs/07-design-brief.md, docs/05-pages.md
**Figma:** file `BookShelf Design`, page `Design System`
**Date:** 16 Oct 2026

Token names in this file are also the CSS variable names (`primary` becomes `--primary`). Components use tokens only, never raw hex.

## 1. Principles

- Calm and bookish. Warm paper page, white cards, ink text.
- Serif for what you read (titles, descriptions, notes). Sans-serif for what you use (buttons, forms, navigation, labels).
- One main action per screen.
- Status is never shown by color alone.
- Everything is reachable by keyboard.
- Three devices, designed mobile first: mobile 390, tablet 768, laptop and PC 1440.

## 2. Colors

Palette made with the 9 shade HSB arc method, 7 scales in Figma. Final tokens were chosen after WebAIM contrast tests. Only the 28 tokens below are used. The page is `bg`, cards are `surface` (white), and `primary-light` is only for small tints.

### 2.1 Primary: buttons, links, focus

| Token | Hex | Used for |
|---|---|---|
| `primary-light` | `#EBF7FF` | Selected items, hover background, active menu row, Want to read pill, initial avatar |
| `primary-focus` | `#1A4866` | Focus ring, Want to read pill border |
| `primary` | `#0B334D` | Buttons, links, active nav, current page |
| `primary-dark` | `#00253D` | Button hover and pressed |

### 2.2 Secondary (wine): decoration only

| Token | Hex | Used for |
|---|---|---|
| `secondary-light` | `#FFEBF4` | Soft decorative background |
| `secondary-focus` | `#661A3F` | Decorative icons, highlights |
| `secondary` | `#4D0B2B` | Logo bar, dividers, empty state art |
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

Fully rounded. All three have the same 1 px border, so they keep the same size.

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

| Style (Figma name) | Font | Weight | Size / line | Used for |
|---|---|---|---|---|
| `Heading/Display` | Gentium Book Plus | 700 | 44 / 52 | Landing hero, error number (tablet and laptop and PC) |
| `Heading/Display Mobile` | Gentium Book Plus | 700 | 36 / 44 | Same, on mobile |
| `Heading/H1` | Gentium Book Plus | 700 | 32 / 40 | Page titles (tablet and laptop and PC) |
| `Heading/H1 Mobile` | Gentium Book Plus | 700 | 28 / 36 | Page titles on mobile |
| `Heading/H2` | Gentium Book Plus | 700 | 24 / 32 | Section headings, stat numbers, error message |
| `Heading/H3` | Gentium Book Plus | 700 | 20 / 28 | Book title on cards, modal title, wordmark, Security row title |
| `Reading/Regular` | Gentium Book Plus | 400 | 17 / 28 | Book description, notes |
| `Body/Regular` | Inter | 400 | 16 / 24 | Inputs, alert text, UI text |
| `Body/Medium` | Inter | 500 | 16 / 24 | Form labels, nav links, menu items |
| `Body/Small` | Inter | 400 | 14 / 20 | Author, hints, form errors, footer, meta lines |
| `Button` | Inter | 600 | 16 / 20 | Button text |
| `Label` | Inter | 500 | 12 / 16 | Pills, genre, page numbers, progress numbers. Uppercase, letter spacing 4% |
| `Code` | Inter | 600 | 24 / 32 | Digits in the code boxes. Letter spacing 0, because each digit sits in its own box |

Only mobile uses the `Mobile` heading styles. Tablet uses the desktop sizes.

### 3.3 Type rules

- One H1 per page. Do not skip heading levels in HTML. Choose by meaning, not size.
- Gentium Book Plus has only weights 400 and 700. Headings are always 700.
- `Reading/Regular` is for long text only (descriptions, notes). Interface text is always Inter.
- `Label` is for short labels and numbers, never sentences.
- Interface text is never below 14 px. Inputs are 16 px so phones do not zoom.
- Long text stays 60 to 75 characters per line, with a maximum width of 720.
- Copy style: only the first word of a button, link, label, or message is capitalized (`Forgot password?`, `Save book`).

## 4. Spacing, shape, depth

| Item | Values |
|---|---|
| Spacing scale (px) | 4, 8, 12, 16, 24, 32, 48, 64. Padding, gaps, and margins use only these |
| Radius | `sm` 4 (inputs, code boxes, checkbox), `md` 8 (cards, buttons, alerts, modal), `full` 999 (pills, progress bar, avatars) |
| Border | 1 px `border` (cards, dividers), 1 px `border-strong` (inputs) |
| Shadow `sm` | `0 1px 2px rgba(38,36,31,0.08)`, cards |
| Shadow `md` | `0 4px 12px rgba(38,36,31,0.12)`, modal, card hover |
| Focus ring | 2 px gap in `bg`, then a 2 px ring in `primary-focus`, on every clickable item |

## 5. Layout and devices

### 5.1 Devices

| | Mobile | Tablet | Laptop and PC |
|---|---|---|---|
| Width | up to 599 px | 600 to 899 px | 900 px and up |
| Figma frame | 390 | 768 | 1440 |
| Frame names | `Mobile/...` | `Tablet/...` | `Desktop/...` |
| Grid | 4 columns, gutter 16, margin 16 | 8 columns, gutter 24, margin 32 | 12 columns, gutter 24, margin 160, content max 1120 |
| Page padding, top and bottom | 24 | 32 | 48 |
| Headings | Mobile styles | desktop styles | desktop styles |
| Navbar | height 56, hamburger | height 64, full links | height 64, full links |
| Library grid | 1 column | 2 columns | 3 columns |
| Profile numbers | 2 columns | 3 columns | 6 in one row |

### 5.2 Layout rules

- Mobile first. CSS base styles are for phones, with `min-width` media queries at 600 and 900 px.
- Wide screens keep the content centered at a maximum width of 1120.
- Auth cards: width Fill on mobile. On tablet and laptop and PC they are 420 wide, centered.
- Forms with a cover: one column on mobile, and two columns (cover at the left) on tablet and laptop and PC. Maximum form width 880.
- No horizontal scrolling from 320 px up. Touch targets are at least 44 px.
- The `Device` property of a component has three values: Mobile, Tablet, Desktop. Where the Tablet spec equals the Desktop spec, still keep the Tablet variant, so a screen can use one property on every device.

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
| `Icon/cross` | x | Close alert, modal, and menu |
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
| Navbar menu button | 20 | `text` |
| No-cover placeholder | 32 | `text-soft` |
| Cover upload | 24 | `text-soft` |
| Feature card icon | 20 | `primary` |
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
- Use `stroke="currentColor"` and no fixed width and height in the SVG, so CSS controls color and size. The pages use the icons as inline SVG.
- The Google logo SVG keeps its fixed colors.

## 7. Components

### 7.1 Global rules

- Auto layout on every component. Width is Fill unless a spec says otherwise.
- Colors come from variables, text from text styles, spacing from the scale in section 4.
- Names are `Group/Name`. Variant properties are in Title Case: `State`, `Style`, `Type`, `Device`.
- `Device` has three values (Mobile, Tablet, Desktop) and exists only on components whose layout differs.
- Focus ring: two drop shadows, X 0, Y 0, Blur 0. First spread 2 in `bg`, second spread 4 in `primary-focus`.
- Touch targets are at least 44 px on mobile.
- A skip link ("Skip to content") is the first focusable item on every page. It is hidden until focused, then shows at the top left as a Primary button.

### 7.2 Actions

| Component | Spec | Variants |
|---|---|---|
| **Button** | Height 44, padding 12 top and bottom, 24 sides, gap 8, radius `md`. Text `Button`. Optional left icon (20). Width Hug or Fill. Primary: `primary` fill, `text-on-primary` text. Outline: `surface` fill, 1 px `border-strong`, `text` text. Danger: `error` fill, `text-on-primary` text, Delete only. The Google button is Outline with `Icon/google`. Properties: Label, IconLeft, Icon, Width | Style: Primary, Outline, Danger. State: Default, Hover (Primary `primary-dark`, Danger `error-dark`, Outline `bg-muted`), Pressed (same as Hover), Focus (ring), Disabled (`bg-muted` fill, `text-soft` text, 1 px `border`), Loading (spinner, label `Saving...`) |
| **Button/Icon** | 40 x 40, radius `md`, icon 20. Icon `text-soft`, Danger icon `error`. Hover fill `bg-muted` (Danger `error-light`). Disabled icon `border`. On mobile the tap area is 44 x 44. Property: Icon | Type: Default, Danger. State: Default, Hover, Focus, Disabled |
| **Link** | `Body/Medium` or `Body/Small`. Inline `primary`. Nav `text`, hover `primary`. Nav Active `primary` with an underline (offset 4). Property: Label | Type: Inline, Nav, Nav Active. Size: Medium, Small. State: Default, Hover (underline), Focus (ring) |

### 7.3 Forms

| Component | Spec | Variants |
|---|---|---|
| **Input/Text** | Label above (`Body/Medium`, `text`). Field height 44, padding 12 sides, gap 8, radius `sm`, `surface` fill, 1 px `border-strong`, value `Body/Regular`. Message below (`Body/Small`): a hint is `text-soft`. An error is `error-dark` with `Icon/error` (16), and replaces the hint. Optional left icon (20). Properties: Label, Placeholder, Message, ShowLabel, ShowMessage, IconLeft | State: Default, Hover (border `text-soft`), Focus (border `primary` plus ring), Filled, Error (border `error`), Disabled (`bg-muted` fill, `border` stroke) |
| **Input/Password** | Input/Text plus an eye icon (20, `border-strong`) at the right, swapped between `eyeOpen` and `eyeClosed`. Filled shows dots | Same as Input/Text |
| **Input/Select** | Input/Text plus `Icon/chevronDown` at the right. The open list is the browser's native select | Same as Input/Text |
| **Input/Textarea** | Input/Text with min height 96, padding 12 on all sides, text aligned top. Optional counter `0 / 2000` (`Body/Small`, `text-soft`) at the right of the message row. Property: ShowCounter | Same as Input/Text |
| **OTP/Box** | Height 56, width Fill (min 40), radius `sm`, `surface` fill, 1 px `border-strong`. One digit in `Code`, centered. Caret in `primary` when focused. Property: Digit | State: Empty, Focus (border `primary` plus ring), Filled, Error (border `error`), Disabled |
| **Input/OTP** | The code input. Label, a row of 6 `OTP/Box` with gap 8 and width Fill, then the message row. Properties: Label, Message, ShowMessage | State: Default, Focus, Typing, Filled, Error, Disabled |
| **Checkbox** | Box 20 x 20, radius `sm`, `surface` fill, 1 px `border-strong`. Label `Body/Regular` at the right, gap 8. Checked: `primary` fill with `Icon/check`. Property: Label | State: Unchecked, Checked, Focus (ring on the box), Disabled |
| **Divider/Text** | Two 1 px `border` lines with `or` (`Body/Small`, `text-soft`) between, gap 12. Property: Label | Default |
| **Cover Upload** | Label `Cover`. Dashed 1 px `border-strong` box, radius `md`, padding 24, `surface` fill. Empty: `Icon/upload`, `Choose a cover image`, hint `JPG, PNG or WEBP, max 2 MB`. Filled: preview 2:3 (mobile 120 x 180 centered, tablet 200 x 300, desktop 240 x 360, radius `sm`) and an Outline `Replace` button, plus `Remove` on edit | State: Empty, Filled, Error. Device |
| **Form/Actions** | Cancel (Outline) and Save (Primary). Mobile: vertical, gap 12, both Fill, Save on top. Tablet and Desktop: horizontal, right aligned, gap 12. Property: PrimaryLabel | Device |

### 7.4 Feedback

| Component | Spec | Variants |
|---|---|---|
| **Alert** | Radius `md`, 1 px status border, status light fill, padding 12 top and bottom, 16 sides, gap 12, top aligned. Status icon (20, status default color). Text `Body/Regular` in status dark. Close is a `Button/Icon` with `Icon/cross`. Width Fill, max 640. Properties: Message, ShowClose | Type: Success (`Icon/verified`), Info (`Icon/info`), Warning (`Icon/warning`), Error (`Icon/error`) |
| **Modal/Confirm** | Panel: `surface`, radius `md`, shadow `md`, padding 24, gap 16, width Fill, max 400. Title `Heading/H3`, message `Body/Regular`. The overlay is `text` at 50%, added per screen. Mobile: buttons stacked, Fill, Delete (Danger) on top. Tablet and Desktop: Cancel (Outline) and Delete (Danger), right aligned. Properties: Title, Message, ConfirmLabel | Device |
| **Badge/Status** | Status pill: height 24, padding 4 top and bottom, 12 sides, radius `full`, 1 px border, `Label` text. Colors from 2.5 | Type: Want to read, Reading, Finished |
| **Progress/Bar** | Track height 8, radius `full`, `bg-muted`. Fill `primary`, or `success` at 100%. Percent in `Label`, fixed width 40, right aligned, `text-soft` (`success-dark` when complete). Width Fill. Property: Percent | State: Empty, Partial, Complete |

### 7.5 Navigation and layout

| Component | Spec | Variants |
|---|---|---|
| **Navbar/Visitor** | `surface` fill, 1 px `border` at the bottom. Wordmark: a 4 x 20 bar in `secondary` (radius 2), gap 8, `BookShelf` in `Heading/H3` (the logo symbol replaces the bar when ready). Mobile: 390 x 56, side padding 16, right side `Button/Icon` with `Icon/menu`. Tablet: 768 x 64, side padding 32. Desktop: 1440 x 64, side padding 160. Tablet and Desktop right side: Link `Log in` (Nav) and Primary button `Register`, gap 24 | Device |
| **Navbar/User** | Same frame. Tablet and Desktop: Links `My Library` and `Add Book` (the current one is Nav Active), then at the right a Link (Nav) holding a small Avatar and the username (gap 8, to the profile) and an Outline `Logout`. Mobile: wordmark and menu icon | Device. Active: My Library, Add Book, Profile, None |
| **Menu/Item** | Height 48, padding 0 top and bottom, 16 sides, `Body/Medium`, `text`. Active: `primary-light` fill, `primary` text. Width Fill. Property: Label | State: Default, Active, Focus |
| **Menu/Mobile** | Mobile only. Opens under the navbar. Width Fill, `surface`, bottom 1 px `border`, padding 8 top and bottom. Visitor: `Log in`, `Register`. User: `My Library`, `Add Book`, then a row with a small Avatar and the username (to the profile), and an Outline `Logout` (Fill) | Type: Visitor, User |
| **Footer** | `BookShelf 2026`, `Body/Small`, `text-soft`, centered, padding 24 | Default |
| **Page Header** | Title `Heading/H1` (mobile `Heading/H1 Mobile`), optional subtitle (`Body/Regular`, `text-soft`), optional Primary button. Mobile: stacked, gap 16, button Fill. Tablet and Desktop: one row, space between. Properties: Title, Subtitle, ShowSubtitle, ShowButton, ButtonLabel | Device |
| **Pagination** | Items 40 x 40, radius `sm`, `Label` text. Current: `primary` fill, `text-on-primary`. Others: `primary` text, hover `primary-light`. Prev and Next: height 40, 1 px `border-strong`, `Body/Medium`, chevron 16. Disabled: `bg-muted` fill, `border` stroke, `text-soft`. Mobile: Prev, `PAGE 2 OF 3`, Next. Tablet and Desktop: Prev, numbers with `...` gaps, Next | Device. Position: First, Middle, Last |

### 7.6 Library and book content

| Component | Spec | Variants |
|---|---|---|
| **Toolbar/Library** | Search `Input/Text` (left icon search, placeholder `Title or author`), status and genre `Input/Select`, Link `Clear` (Inline, Small). Mobile: vertical, gap 16: search, then two selects side by side, then Clear. Tablet: search on row 1, then status, genre, and Clear on row 2. Desktop: one row, gap 16, selects 200 wide. No Apply button. Loading: a spinner replaces the search icon | Device. State: Default, Loading |
| **Card/Book** | `surface`, 1 px `border`, radius `md`, shadow `sm`, padding 16. Details (gap 8): title `Heading/H3` (max 2 lines), author `Body/Small` (`text-soft`), genre `Label` (`text-soft`), Badge/Status, Progress/Bar. Mobile: horizontal card, cover 88 x 132 at the left. Tablet and Desktop: vertical card with a cover panel (`bg-muted`, height 264, padding 12) holding the cover 160 x 240 (radius `sm`). No cover: `Icon/book` (32, `text-soft`) on `bg-muted` | Device. State: Default, Hover (shadow `md`), Focus. Cover: Image, None |
| **Book/Header** | Mobile: vertical and centered, cover 160 x 240, title `Heading/H1 Mobile`, buttons side by side and Fill. Tablet: horizontal, gap 32, cover 200 x 300. Desktop: horizontal, gap 32, cover 240 x 360. Details: title `Heading/H1`, author `Body/Regular` (`text-soft`), genre `Label` and pill, `Pages 128 / 300`, Progress/Bar (max 360), dates (`Body/Small`, `text-soft`), Outline `Edit` and Danger `Delete` | Device |
| **Card/Note** | `surface`, 1 px `border`, radius `md`, padding 16, gap 16. Text `Reading/Regular`. Meta row: `PAGE 42` (`Label`, `text-soft`), date and `edited` (`Body/Small`, `text-soft`). Right: `Button/Icon` edit and Danger delete. Properties: Text, Page, Date, ShowEdited | Default |
| **Form/Note** | `Input/Textarea` (Label `Add a note`, counter on), `Input/Text` for the page (width 120), Primary `Add note`. Mobile: stacked, button Fill. Tablet and Desktop: the page and the button on one row | Device |

### 7.7 Profile

| Component | Spec | Variants |
|---|---|---|
| **Avatar** | Circle (radius `full`). Image fills the circle. Initial: first letter of the username in uppercase on a `primary-light` fill with a 1 px `border`, text `primary`. Small 32 uses `Body/Medium`, Medium 64 uses `Heading/H2`, Large 96 uses `Heading/H1` | Size: Small, Medium, Large. Type: Image, Initial |
| **Profile/Header** | Large Avatar, username (`Heading/H1`, mobile `Heading/H1 Mobile`), email (`Body/Regular`, `text-soft`), meta line (`Body/Small`, `text-soft`): `Member since Oct 2026` and `Signed in with Google`. Outline `Edit profile`. Mobile: vertical and centered, button Fill. Tablet and Desktop: horizontal, gap 24, button at the right | Device |
| **Card/Stat** | `surface`, 1 px `border`, radius `md`, padding 16, gap 4. Number `Heading/H2`, label `Body/Small` in `text-soft`. As a link: hover shadow `md`, focus ring. Width Fill. Grid: 2 columns on mobile, 3 on tablet, 6 on desktop | Type: Static, Link. State: Default, Hover, Focus |
| **Avatar Upload** | Outline `Choose photo` (`Add photo` on register) and Outline `Remove`, hint `JPG, PNG or WEBP, max 2 MB` (`Body/Small`, `text-soft`), error in `error-dark` with `Icon/error`. Full (edit profile): Large avatar. Mobile: centered, buttons stacked and Fill. Tablet and Desktop: avatar at the left, buttons and hint at the right. Compact (register): Medium avatar at the left, `Add photo` and the hint at the right, and a `Remove` link after a file is chosen | Layout: Full, Compact. State: Default, Error. Device |
| **Profile/Security** | `surface`, 1 px `border`, radius `md`, padding 24. Left: `Password` (`Heading/H3`) and a line (`Body/Small`, `text-soft`). Right: Outline `Change password` (`Set password` for a Google-only account). Mobile: vertical, button Fill. Tablet and Desktop: horizontal, space between | Device |

### 7.8 Page level

| Component | Spec | Variants |
|---|---|---|
| **Card/Auth** | Container for login, register, verify, forgot, and reset. `surface`, 1 px `border`, radius `md`. Header: title `Heading/H1` (mobile `Heading/H1 Mobile`) and an optional subtitle (`Body/Regular`, `text-soft`), gap 8, then the content area (gap 16). Mobile: width Fill, padding 24, gap 24, no shadow. Tablet and Desktop: width 420, padding 32, gap 24, shadow `sm`. Properties: Title, Subtitle, ShowSubtitle | Device |
| **Card/Feature** | Landing feature. Icon in a 40 px circle (`primary-light`, icon `primary`), title `Heading/H3`, text `Body/Regular` (`text-soft`). Padding 24, gap 12, `surface`, 1 px `border`, radius `md`. Properties: Icon, Title, Text | Default |
| **Empty State** | Art in `secondary` tones (mobile 120, tablet and desktop 160), title `Heading/H3`, one line `Body/Regular` (`text-soft`), optional button. No books: Primary `Add a book`. No results: Outline `Clear filters`. No notes: no button | Type: No books, No results, No notes. Device |
| **Error State** | Vertical, centered, gap 16. Status number in `Heading/Display` (mobile `Heading/Display Mobile`), message in `Heading/H2`, Primary `Back to home`. Padding 48 top and bottom on tablet and desktop, 32 on mobile. 404: `This page was not found.` 500: `Something went wrong. Try again later.` Properties: Code, Message | Type: 404, 500. Device |

## 8. Accessibility

- Touch targets at least 44 px on mobile.
- Every field has a visible label. Errors sit under the field and are tied to it.
- Visible focus ring on every clickable item.
- Nothing relies on color alone.
- The code input: each box is labeled (`Digit 1 of 6`), and pasting a code fills all six.
- The mobile menu button has an accessible name and shows whether the menu is open.
- Flash messages are announced to screen readers. Closing the modal returns focus to the button that opened it.
- Motion: only the spinner, hover, and fade. No other animation in v1.

## 9. Figma setup

- Free plan allows 3 pages per file: `Cover`, `Design System`, `Screens`. `Design System` has the sections Colors, Typography, Icons, Components. `Screens` has the sections Mobile, Tablet, and Desktop. The prototype is built on the `Screens` page.
- Color variables are in a collection `Colors`, grouped by family: `primary`, `secondary`, `neutral`, `success`, `warning`, `error`, `info`. Figma `primary/dark` is the token `primary-dark` and the CSS variable `--primary-dark`. The base shade of a family is `default` (`primary/default` is `--primary`). Neutral names drop the group in CSS (`neutral/bg` is `--bg`).
- Text styles are named exactly as in 3.2 (13 styles).
- Components use Auto layout with spacing from the scale only.
- Screen frames are named `Mobile/Login`, `Tablet/Login`, `Desktop/Login`, and so on. State frames add the state: `Mobile/Login Error`.
- Mockups use real content: the messages in `docs/04-routes.md`, and sample books with long and short titles, with and without covers.

## 10. Handoff to code

| Figma | Code |
|---|---|
| Color variables | CSS variables in `tokens.css` (`--primary`, `--bg`, `--text`, and so on). Raw hex exists only there |
| Text styles | Classes and element styles in `base.css`. Sizes in `rem`, so they follow the user's browser setting |
| Spacing, radius, shadows | CSS variables in `tokens.css` |
| Components | Classes in `components.css`. One EJS partial per component in `frontend/views/partials/` |
| `Device` property | Base styles for mobile. `min-width: 600px` for tablet. `min-width: 900px` for laptop and PC |
| Icons | SVG files in `frontend/public/icons/`, used inline with `currentColor` |
| Fonts | Google Fonts link with `display=swap`. Only the listed weights are loaded |
| Frames | Used to compare the built page at 390, 768, and 1440 px |

## 11. Done when

- Variables, text styles, and icons exist in Figma and match this file.
- Every component in section 7 exists with all its variants, including the Tablet variants.
- Each component is used at least once in the mockups.
- Names, properties, and specs in Figma match this file, and any change is made in both places.