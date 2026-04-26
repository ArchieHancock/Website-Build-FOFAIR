# CLAUDE.md — Fair Oaks Income Fund Website

## Project Overview

This project is the public investor website for **Fair Oaks Income Fund**, a
London Stock Exchange listed investment trust. The site must meet the standards
expected of a regulated, publicly listed UK investment vehicle — professional,
trustworthy, compliant, and visually polished.

**Deployment stack:** Local dev → GitHub → Vercel (auto-deploy on push to `main`)
Always write code that works in this pipeline. No local-only dependencies.
All environment variables must be Vercel-compatible.

---

## Input Mode

The user will provide a **website URL** as the design reference.
Do NOT ask for Figma links or screenshots.

On receiving a URL:
1. Fetch and analyse the live site structure, layout, and components
2. Identify every page, section, and UI pattern
3. Extract colour, typography, spacing, and interaction patterns
4. Confirm the page list with the user before writing any code
5. Recreate the layout and structure — replacing all content with Fair Oaks
   content, brand colours, and identity as defined in this file

---

## Site Structure

Build the following pages. Every page must be reachable from the main nav.
Hash-based routing (`#/home`, `#/about` etc.) unless user requests React Router.

| Route              | Page                   |
|--------------------|------------------------|
| `#/`               | Home                   |
| `#/about`          | About Us               |
| `#/reports`        | Reports                |
| `#/documentation`  | Documentation          |
| `#/media`          | Media                  |
| `#/contact`        | Contact                |
| `#/faqs`           | FAQs                   |
| `#/key-risks`      | Key Risks              |

---

## Disclaimer Gate

**The entire website must be gated behind a disclaimer modal.**

- On first visit, show a full-screen disclaimer overlay before any content loads
- The disclaimer must be accepted before the user can access any page
- Store acceptance in `sessionStorage` (resets each browser session)
- If not accepted, the background site content must be blurred or hidden entirely
- Include two buttons: **"I Accept"** (proceeds) and **"I Do Not Accept"**
  (redirects to `https://www.google.com`)
- Disclaimer text must include (adapt as needed):
  > "This website is intended for professional and institutional investors only.
  > The information contained herein does not constitute financial advice and
  > is subject to the Key Risks outlined on this site. Past performance is not
  > a reliable indicator of future results. Fair Oaks Income Fund is a
  > closed-ended investment company listed on the London Stock Exchange."

---

## Brand Identity & Visual Style

### Logo
- Primary logo file: `Fair Oaks Income Limited - Blue/White.svg`
- Use the **Blue version** on white/light backgrounds
- Use the **White version** on navy/dark backgrounds
- Always place the logo in the top-left of the navigation bar
- Maintain aspect ratio — never stretch or crop

### Visual Theme — Oak Tree Inspired
The site aesthetic should feel rooted in nature while remaining institutional.
Think: **solid, enduring, deep-rooted** — like an oak tree.

Apply this through:
- **Hero sections**: Full-width oak tree imagery or subtle oak leaf SVG
  illustrations — either sourced or generated as inline SVG
- **Animations**: Gentle leaf-fall effects, slow parallax on tree imagery,
  subtle branch/leaf SVG animations on scroll or page load
- **Textures**: Light wood-grain or bark-like subtle textures for section
  dividers or backgrounds (CSS only, no heavy images)
- **Icons**: Use oak leaf or acorn motifs for bullet points, section markers,
  and decorative elements where appropriate
- **Section dividers**: Subtle oak leaf SVG dividers between major sections
- **Loading states**: Oak leaf spinner instead of generic loaders

Imagery must feel **premium and institutional** — not rustic or folksy.
Oak tree = strength, longevity, trust. That is the tone.

---

## Colour Palette

Import `colors.css` at the top of every stylesheet.
**Never hardcode hex values** — always use CSS variables.

```css
@import './colors.css';
```

### Full Variable Reference

| Variable                    | Hex       | Usage                                      |
|-----------------------------|-----------|--------------------------------------------|
| `--color-white`             | `#FFFFFF` | Backgrounds, cards, clean space            |
| `--color-navy`              | `#004668` | Primary brand — headings, nav, hero bg     |
| `--color-green`             | `#4DB79D` | CTAs, accents, icons, highlights           |
| `--color-light-blue`        | `#99BBD1` | Subtle highlights, borders, dividers       |
| `--color-pale-blue-gray`    | `#DDE8F0` | Section backgrounds, soft fills            |
| `--color-deep-blue`         | `#558DB2` | Secondary accents, hover states, links     |
| `--color-grey`              | `#BFBFBF` | Placeholder text, disabled states          |
| `--color-grey-stone`        | `#CBCFD4` | Borders, separators, subtle backgrounds    |
| `--color-light-grey`        | `#F2F2F2` | Page background, alternate row fills       |
| `--color-dark-grey`         | `#7F7F7F` | Body text, secondary text                  |

### Semantic Aliases

| Variable                    | Maps To                  | Usage                       |
|-----------------------------|--------------------------|-----------------------------|
| `--color-bg-page`           | `--color-light-grey`     | Main page background        |
| `--color-bg-section-alt`    | `--color-pale-blue-gray` | Alternate section bg        |
| `--color-bg-card`           | `--color-white`          | Card / panel background     |
| `--color-text-primary`      | `--color-navy`           | Headings, key text          |
| `--color-text-body`         | `--color-dark-grey`      | Body / paragraph text       |
| `--color-text-muted`        | `--color-grey`           | Captions, hints             |
| `--color-text-on-dark`      | `--color-white`          | Text on dark backgrounds    |
| `--color-accent-primary`    | `--color-green`          | Main CTAs, badges           |
| `--color-accent-secondary`  | `--color-deep-blue`      | Hover states, links, icons  |
| `--color-border`            | `--color-grey-stone`     | Input borders, dividers     |
| `--color-border-light`      | `--color-light-blue`     | Subtle borders              |

### Colour Usage Rules
- Navy (`--color-navy`) is the dominant brand colour — use for nav, hero
  backgrounds, and primary headings
- Green (`--color-green`) is the primary accent — use sparingly for CTAs,
  active states, and key highlights
- Deep Blue (`--color-deep-blue`) is the secondary accent — hover states,
  inline links, secondary buttons
- Never use more than 3 colours in a single component
- The overall feel must be: **calm, clean, credible, institutional**

---

## Typography

- **Headings**: A serif or refined sans-serif — something with gravitas.
  Suggested: `Playfair Display`, `Cormorant Garamond`, or `DM Serif Display`
  (loaded via Google Fonts CDN)
- **Body**: Clean, readable — `DM Sans`, `Lato`, or `Source Sans 3`
- **Data / figures**: Monospaced for NAV tables and financial figures:
  `IBM Plex Mono` or `Roboto Mono`
- Font sizes must be consistent — define a type scale, do not use arbitrary values

---

## Technical Defaults

- **Tailwind CSS** via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- **Single `index.html`** unless user requests React/Vite scaffold
- **Hash-based routing** (`#/home`, `#/about`) — zero build step
- **Placeholder images** from `https://placehold.co/` when real images absent
- **Icons**: Lucide via CDN or inline SVG — no icon font dependencies
- **Responsive**: Mobile-first (sm → md → lg breakpoints)
- **No build-step dependencies** unless explicitly requested
- All code must be **Vercel-deployable** — push to GitHub, auto-deploy works
  without any extra configuration

---

## Deployment Workflow

```
Claude Code edits files locally
        ↓
git add . && git commit -m "description"
        ↓
git push origin main
        ↓
Vercel auto-detects push → builds → deploys
        ↓
Live at yourproject.vercel.app (or custom domain)
```

- Do not introduce dependencies that break this pipeline
- Environment variables go in Vercel dashboard → Settings → Environment Variables
- Never hardcode secrets or API keys in source files

---

## Build Workflow

### Phase 1 — Understand the Reference Site
1. Fetch the provided URL and analyse structure, layout, and components
2. List every page, section, nav item, and interactive element found
3. Note spacing, typography, colour patterns, and interaction behaviours
4. Confirm page list with user before writing any code

### Phase 2 — Scaffold the Project
1. Generate `index.html` with Tailwind CDN, vanilla JS router, disclaimer gate
2. Build navigation with all 8 pages linked and working
3. Apply Fair Oaks colour palette and typography throughout
4. Add logo to nav using the SVG file: `Fair Oaks Income Limited - Blue/White.svg`

### Phase 3 — Screenshot & Compare
1. Screenshot each rendered page
2. Compare against reference URL layout (layout only — content will differ)
3. List every layout/spacing mismatch explicitly as a numbered list

### Phase 4 — Fix, Re-screenshot, Repeat
1. Fix all listed mismatches
2. Re-screenshot and compare
3. Repeat until visually within ~2–3px of reference layout on all pages
4. **Always complete at least 2 full comparison rounds before stopping**

---

## Interactivity Rules

- Every nav link must work — no dead links
- Disclaimer modal: blocks all content until accepted
- FAQs: accordion — click to expand/collapse each item
- Key Risks: expandable risk sections with clear regulatory warning banner
- Contact: form with validation and success/error feedback
- Reports & Documentation: filterable document list (by year / type)
- Media: filterable news/press grid
- All hover, focus, and active states must be styled
- Mobile hamburger menu must work on narrow viewports

---

## Content & Compliance Rules

- Do not invent financial data, NAV figures, or performance numbers
- Use clearly labelled placeholder text for all financial content
- Key Risks page must include a prominent regulatory warning banner at the top
- All pages must include a footer with: legal disclaimer, LSE listing notice,
  and links to Key Risks and FAQs
- Do not "improve" the reference design unsolicited — match layout,
  substitute Fair Oaks content and brand

---

## Output Rules

- No sycophantic preamble — analyse and proceed
- Report mismatches as a numbered list before fixing
- Code blocks only — no prose wrapping around file contents
- Never output placeholder comments like `// TODO: add logic here`
- Specific mismatch reporting: "heading is 32px, reference shows 24px" — not "font looks off"
- If a step fails, stop immediately and report the full error before attempting a fix