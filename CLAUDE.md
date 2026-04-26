# Website Builder — Screenshot & Figma to Functional Site

## Input Modes

This tool accepts two input types. Detect which one is provided and follow the correct workflow.

**Mode A — Screenshot(s)**
User provides one or more images of a design. May include CSS notes or style tokens.

**Mode B — Figma Link**
User provides a Figma file/frame URL. Use the Figma MCP (`/figma-use`) to extract layout, components, colours, and spacing before writing any code.

If neither is provided, ask: "Please share a screenshot or a Figma link to get started."

---

## Workflow

### Phase 1 — Understand the Design

**If Screenshot:**
1. Identify every distinct page or view visible across all images.
2. List UI elements: nav, tabs, modals, buttons, forms, cards, footers.
3. Note colour values (hex), font sizes, spacing (px), and border radii.
4. Ask the user to confirm the page list before writing code.

**If Figma link:**
1. Fetch the file via Figma MCP. Extract frames, components, and design tokens.
2. Map each Figma frame to a page/route.
3. Extract exact colours, typography, and spacing from variables/styles.
4. Ask the user to confirm the page list before writing code.

---

### Phase 2 — Scaffold the Project

Generate a single `index.html` with:
- Tailwind CSS via CDN
- Vanilla JS router (hash-based `#/page` routing — no build step required)
- All pages rendered as sections, toggled by the router
- Full navigation bar with working links for every page
- Functional tabs, accordions, modals, and dropdowns (no placeholder `TODO` code)

If the user requests React/Vite instead, scaffold with:
```
npm create vite@latest . -- --template react
npm install -D tailwindcss
```
One component file per page. React Router for navigation.

---

### Phase 3 — Screenshot & Compare

1. Screenshot the rendered page:
   ```
   npx puppeteer-screenshot index.html --fullpage
   ```
   If multi-section, screenshot each page/tab individually too.

2. Compare your screenshot against the reference (image or Figma frame). Check:
   - Spacing and padding (measure in px)
   - Font sizes, weights, and line heights
   - Colours (exact hex values)
   - Alignment and positioning
   - Border radii, shadows, box models
   - Responsive breakpoints
   - Icon/image sizing and placement
   - Interactive states (hover, active, focus)

3. List every mismatch explicitly before fixing anything.

---

### Phase 4 — Fix, Re-screenshot, Repeat

4. Fix every listed mismatch. Edit HTML/Tailwind/JS directly.
5. Re-screenshot and compare again.
6. Repeat steps 3–5 until visually within ~2–3px of the reference on all pages.

**Do NOT stop after one pass. Always complete at least 2 full comparison rounds.**
Only stop when the user says so or when no visible differences remain across all pages.

---

## Multi-Page & Interactivity Rules

- Every page listed in the nav must be reachable by click — no dead links.
- Buttons must do something: open a modal, navigate, submit a form, toggle state. No empty `onclick=""`.
- Tabs must switch content on click.
- Forms must validate on submit and show feedback (success/error state).
- Modals must open and close cleanly.
- Mobile hamburger menu must work on narrow viewports.
- All interactive states (hover, focus, disabled) must be styled.

---

## Figma-Specific Rules

- Extract design tokens (colours, spacing, radius, typography) via MCP before writing any CSS.
- Map Figma auto-layout to Tailwind flex/grid classes.
- Respect Figma component variants — implement all states shown.
- If Figma annotations are present, apply them verbatim.
- Do not invent layout that isn't in the Figma file.

---

## Technical Defaults

- Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Placeholder images from `https://placehold.co/` when source images are absent
- Hash-based routing (`#/home`, `#/about`, etc.) for zero-build multi-page
- Mobile-first responsive design (sm → md → lg breakpoints)
- Single `index.html` unless the user requests a build tool
- Icons: Lucide via CDN or inline SVG — no icon font dependencies

---

## Output Rules

- Do not add sections, features, or content not visible in the reference
- Match the reference exactly — do not "improve" the design unsolicited
- If the user provides CSS classes or style tokens, use them verbatim
- Keep code clean; inline Tailwind classes are fine, no over-abstraction
- When reporting mismatches, be specific: "heading is 32px but reference shows 24px", not "font looks off"
- Never output placeholder comments like `// TODO: add logic here`
- No sycophantic preamble — report findings and proceed

---

## Response Format (token efficiency)

- Skip greetings and sign-offs
- Report mismatches as a numbered list, then fix silently
- No re-explaining what was already in the prompt
- Code blocks only — no prose wrapping around file contents
- If a step fails, stop immediately and report the full error before attempting a fix