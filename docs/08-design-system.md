# Design System: BookShelf v1

**Source:** docs/07-design-brief.md, Figma file `BookShelf` (page: Design System)
**Date:** 7 Oct 2026

## 1. Colors

Palette made with the 9 shade HSB arc method: 6 scales, 54 shades. Only the 28 tokens below are used in the app. Components and CSS use token names, never raw hex.

### 1.1 Tokens

**Primary (ink blue): buttons, links, focus**

| Token | Hex | Used for |
|---|---|---|
| `primary-light` | `#EBF7FF` | Selected items, hover background |
| `primary-focus` | `#3B6E8F` | Focus ring |
| `primary` | `#2D5F80` | Buttons, links, active nav, current page |
| `primary-dark` | `#1A4866` | Button hover and pressed |

**Secondary (wine): decoration only**

| Token | Hex | Used for |
|---|---|---|
| `secondary-light` | `#FFEBF4` | Soft decorative background |
| `secondary-focus` | `#A0607F` | Decorative icons, small highlights |
| `secondary` | `#802D55` | Logo accent, dividers, empty state art |
| `secondary-dark` | `#661A3F` | Decorative text accents |

**Neutral (warm gray): most of the interface**

| Token | Hex | Used for |
|---|---|---|
| `bg` | `#F4F2EE` | Page background |
| `bg-muted` | `#E8E5DF` | Hover and disabled backgrounds |
| `border` | `#D5D1C9` | Card borders, dividers |
| `border-strong` | `#8C8781` | Input borders, icons |
| `text-soft` | `#6E6A64` | Hints, secondary text |
| `text` | `#26241F` | Main text |
| `surface` | `#FFFFFF` | Cards, inputs, modal |
| `text-on-primary` | `#FFFFFF` | Text on primary buttons |

**Status colors: light is the background, middle is the icon and border, dark is the text**

| Token | Hex | Used for |
|---|---|---|
| `success-light` | `#EBFFF1` | Success alert and Finished badge background |
| `success` | `#358550` | Success icon, border |
| `success-dark` | `#1A6633` | Success text |
| `warning-light` | `#FFF8EB` | Warning background |
| `warning` | `#A8803B` | Warning icon, border |
| `warning-dark` | `#664A1A` | Warning text |
| `error-light` | `#FFECEB` | Error background |
| `error` | `#A1453D` | Error icon, border, Delete button |
| `error-dark` | `#66201A` | Error text, form errors |
| `info-light` | `#EBFFFF` | Info alert and Reading badge background |
| `info` | `#3B9494` | Info icon, border |
| `info-dark` | `#1A6666` | Info text |

### 1.2 CSS variables

```css
:root {
  /* primary */
  --primary-light: #EBF7FF;
  --primary-focus: #3B6E8F;
  --primary: #2D5F80;
  --primary-dark: #1A4866;

  /* secondary (decoration only) */
  --secondary-light: #FFEBF4;
  --secondary-focus: #A0607F;
  --secondary: #802D55;
  --secondary-dark: #661A3F;

  /* neutral */
  --bg: #F4F2EE;
  --bg-muted: #E8E5DF;
  --border: #D5D1C9;
  --border-strong: #8C8781;
  --text-soft: #6E6A64;
  --text: #26241F;
  --surface: #FFFFFF;
  --text-on-primary: #FFFFFF;

  /* status */
  --success-light: #EBFFF1;
  --success: #358550;
  --success-dark: #1A6633;
  --warning-light: #FFF8EB;
  --warning: #A8803B;
  --warning-dark: #664A1A;
  --error-light: #FFECEB;
  --error: #A1453D;
  --error-dark: #66201A;
  --info-light: #EBFFFF;
  --info: #3B9494;
  --info-dark: #1A6666;
}
```

### 1.3 Allowed text pairs and contrast

Rule: 4.5 to 1 for text, 3 to 1 for borders, icons, and focus rings. Ratios below are approximate. Replace with your Stark numbers if they differ.

| Text | On | Ratio | Result |
|---|---|---|---|
| `text` | `bg`, `surface`, `bg-muted`, `primary-light` | about 13.9 on `bg` | Pass |
| `text-soft` | `bg`, `surface` only | about 4.8 on `bg` | Pass |
| `text-soft` | `bg-muted` | about 4.3 | Not allowed |
| `text-on-primary` | `primary`, `primary-dark` | about 6.9 on `primary` | Pass |
| `text-on-primary` | `secondary`, `secondary-dark` | about 8.7 on `secondary` | Pass |
| `primary` (links) | `bg`, `surface` | about 6.1 on `bg` | Pass |
| `success-dark` | `success-light` | about 6.6 | Pass |
| `warning-dark` | `warning-light` | about 7.8 | Pass |
| `error-dark` | `error-light`, `bg`, `surface` | above 7 | Pass |
| `info-dark` | `info-light` | about 6.4 | Pass |
| `border-strong` | `surface` | about 3.6 | Pass (3 needed) |
| `primary-focus` | `bg`, `surface` | about 5.5 on `surface` | Pass (3 needed) |
| status icon colors | their `-light` background | about 4.2 on `success-light` | Pass (3 needed) |

### 1.4 Status badges

| Badge | Background | Text |
|---|---|---|
| Want to read | `bg-muted` | `text` |
| Reading | `info-light` | `info-dark` |
| Finished | `success-light` | `success-dark` |

Every badge shows a text label, never color alone.

### 1.5 Rules

- Components use tokens only. No raw hex in CSS outside the `:root` block.
- `secondary` is decoration only: logo accent, dividers, empty state art, small highlights. Never for buttons, links, or anything that could look like an error (it sits close to `error` in tone).
- Status is never shown by color alone. Always an icon or a text label too.
- Focus ring: 2 px `primary-focus`, 2 px offset, on every clickable item.
- Disabled controls use `bg-muted` background and `border` border. Disabled text has no contrast requirement, but keep it readable.
- Text on colored backgrounds comes only from the allowed pairs in 1.3.

## 2. Typography

### 2.1 Fonts

| Role | Font | Weights | Used for |
|---|---|---|---|
| Heading | Fraunces (serif) | 600 | Page titles, section headings, book titles |
| Body | Work Sans (sans-serif) | 400, 500, 600 | Body text, buttons, forms, navbar, cards |
| Label | IBM Plex Mono (monospace) | 500 | Small labels, status badges, page numbers, progress numbers |

All three are free on Google Fonts.

### 2.2 Load fonts

Add to the layout `<head>` before the stylesheet:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@600&family=IBM+Plex+Mono:wght@500&family=Work+Sans:wght@400;500;600&display=swap" rel="stylesheet">
```

`display=swap` shows text in the fallback font first, so the page is never blank while fonts load.

### 2.3 CSS variables

```css
:root {
  --font-heading: "Fraunces", Georgia, serif;
  --font-body: "Work Sans", system-ui, sans-serif;
  --font-label: "IBM Plex Mono", ui-monospace, monospace;
}
```

### 2.4 Rules

- Headings use `--font-heading`, weight 600 only.
- Body, buttons, inputs, navbar, and card text use `--font-body`.
- `--font-label` is only for short uppercase labels, badges, and numbers. Never for sentences.
- Body text is never below 14 px. Inputs are 16 px so phones do not zoom.
- Long text (book description, notes) stays 60 to 75 characters per line.
- Only the weights listed above are loaded. Do not add more.

### 2.5 Type scale

| Style (Figma name) | Font | Weight | Desktop size / line | Mobile size / line | Used for |
|---|---|---|---|---|---|
| `Heading/Display` | Fraunces | 600 | 40 / 48 | 32 / 40 | Landing page hero only |
| `Heading/H1` | Fraunces | 600 | 32 / 40 | 28 / 36 | Page titles (My Library, Add a book) |
| `Heading/H2` | Fraunces | 600 | 24 / 32 | 24 / 32 | Section headings (Notes, Description) |
| `Heading/H3` | Fraunces | 600 | 20 / 28 | 20 / 28 | Book title on cards, modal title |
| `Body/Regular` | Work Sans | 400 | 16 / 24 | 16 / 24 | Body text, inputs, descriptions, notes |
| `Body/Medium` | Work Sans | 500 | 16 / 24 | 16 / 24 | Form labels, navbar links |
| `Body/Small` | Work Sans | 400 | 14 / 20 | 14 / 20 | Author on cards, hints, error messages, footer |
| `Button` | Work Sans | 600 | 16 / 20 | 16 / 20 | Button text |
| `Label` | IBM Plex Mono | 500 | 12 / 16 | 12 / 16 | Status badges, page numbers, progress, pagination text |

`Label` is uppercase with 4% letter spacing.

Only Display and H1 change on mobile.

### 2.6 CSS

```css
:root {
  --text-display: 2.5rem;    /* 40 */
  --text-h1: 2rem;           /* 32 */
  --text-h2: 1.5rem;         /* 24 */
  --text-h3: 1.25rem;        /* 20 */
  --text-base: 1rem;         /* 16 */
  --text-small: 0.875rem;    /* 14 */
  --text-label: 0.75rem;     /* 12 */

  --leading-display: 3rem;   /* 48 */
  --leading-h1: 2.5rem;      /* 40 */
  --leading-h2: 2rem;        /* 32 */
  --leading-h3: 1.75rem;     /* 28 */
  --leading-base: 1.5rem;    /* 24 */
  --leading-small: 1.25rem;  /* 20 */
  --leading-label: 1rem;     /* 16 */
}

@media (max-width: 600px) {
  :root {
    --text-display: 2rem;      /* 32 */
    --leading-display: 2.5rem; /* 40 */
    --text-h1: 1.75rem;        /* 28 */
    --leading-h1: 2.25rem;     /* 36 */
  }
}

body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-base);
  color: var(--text);
  background: var(--bg);
}

h1, h2, h3, .display {
  font-family: var(--font-heading);
  font-weight: 600;
}

.display { font-size: var(--text-display); line-height: var(--leading-display); }
h1 { font-size: var(--text-h1); line-height: var(--leading-h1); }
h2 { font-size: var(--text-h2); line-height: var(--leading-h2); }
h3 { font-size: var(--text-h3); line-height: var(--leading-h3); }

.text-small { font-size: var(--text-small); line-height: var(--leading-small); }

.label {
  font-family: var(--font-label);
  font-weight: 500;
  font-size: var(--text-label);
  line-height: var(--leading-label);
  text-transform: uppercase;
  letter-spacing: 0.04em;
}
```

Sizes use `rem` so they follow the user's browser font size.

### 2.7 Where each style is used

| Screen part | Style |
|---|---|
| Landing hero | Display |
| Page title | H1 |
| "Notes (3)", "Description" | H2 |
| Book title on card, modal title | H3 |
| Description, notes, form text | Body/Regular |
| Form labels, navbar links | Body/Medium |
| Author, hints, form errors, footer | Body/Small |
| Buttons | Button |
| Status badges, "Page 2 of 5", progress "42%" | Label |
| Flash message text | Body/Regular |

### 2.8 Rules

- Headings do not skip levels in the HTML (H1, then H2, then H3). Choose by meaning, not size.
- One H1 per page.
- Do not use bold or italics on Fraunces. Weight 600 only.
- Text color comes from tokens only (`text`, `text-soft`, status dark colors).
- Do not use `Label` for sentences.