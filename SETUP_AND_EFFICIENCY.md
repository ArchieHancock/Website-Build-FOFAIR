# Claude Code — Setup & Efficiency Guide
## For Website Building (Screenshot → Figma → Functional Site)

---

## 1. Essential MCPs to Install First

These give Claude hands — without them it's guessing.

### Figma MCP (highest priority)
Lets Claude read your actual Figma frames, design tokens, and components directly.
```
# In Claude Code terminal:
/mcp add figma
# Then connect your Figma account when prompted
# Verify it's working:
/figma-use
```
Once connected, you can just paste a Figma URL and Claude extracts colours, spacing, layout, and component states automatically — far more accurate than a screenshot.

### Puppeteer (for screenshot comparison)
```
npm install -g puppeteer
# or per-project:
npm install --save-dev puppeteer
```
The screenshot → compare → fix loop is the core quality mechanism. Without this, comparison is manual and slow.

### Browser MCP (optional but useful)
Lets Claude open your local `index.html` and interact with it — click tabs, open modals, check responsive breakpoints live.

---

## 2. Token Efficiency — The Most Important Optimisations

### Keep CLAUDE.md under 500 tokens
Every line loads on every message. The file you have is about 400 tokens — keep it there. Move detailed reference docs to separate files and reference them with `@path/to/file`.

### Add a `.claudeignore` file
Stops Claude reading irrelevant files and burning context.
```
# .claudeignore
node_modules/
.git/
dist/
*.log
*.map
coverage/
```

### Settings to add to `~/.claude/settings.json`
```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku"
  }
}
```
- `MAX_THINKING_TOKENS: 10000` — default is ~32k, cutting it reduces hidden cost by ~70% on non-complex tasks
- `CLAUDE_CODE_SUBAGENT_MODEL: haiku` — file reading, test running, and exploration tasks use the cheaper model (~80% cost reduction on those steps)

### Use `/compact` before context fills
Don't let sessions auto-compact at 95% — you lose 60–70% of your conversation. Instead, use `/compact` manually at natural breakpoints (e.g. after finishing a page, before starting a new one). Auto-compaction repeatedly = up to 95% of session context gone.

### Use `/clear` between unrelated tasks
Starting a fresh context for a new page is cheaper than dragging a full session along.

### Track spend with `/cost`
Run `/cost` at the end of sessions to see what each type of work costs. After 3–4 sessions you'll know exactly where your tokens go.

---

## 3. Session Habits That Save the Most

| Habit | Why it matters |
|-------|---------------|
| One task per chat session | Context accumulates fast; fresh sessions start lean |
| Point Claude at specific files, not "the whole project" | `"edit index.html line 45"` uses ~10x fewer tokens than `"look at the project"` |
| Use `/compact` at page boundaries | Preserves summary, drops dead context |
| Disable MCPs you're not using | Each active MCP tool description eats tokens from your context window — multiple active MCPs can shrink usable window from 200k to ~70k |
| Give Claude the Figma link, not a screenshot | MCP extraction is more precise and uses fewer correction rounds |

---

## 4. Recommended Project Structure

A lean structure avoids Claude reading things it shouldn't.

```
my-website/
├── CLAUDE.md              ← your base prompt (this file, ~400 tokens)
├── .claudeignore          ← stops Claude reading noise
├── index.html             ← single output file (or src/ if using Vite)
├── docs/
│   ├── design-tokens.md   ← extracted colours, fonts, spacing (referenced by @)
│   └── pages.md           ← list of pages/routes and their purpose
└── reference/
    └── screenshots/       ← your reference images
```

Reference docs from CLAUDE.md like this instead of pasting them inline:
```
See @docs/design-tokens.md for colour and typography values.
See @docs/pages.md for the page structure.
```
Claude pulls these in only when relevant, not on every message.

---

## 5. Useful Slash Commands to Set Up

Save these in `.claude/commands/` — they become `/compare`, `/newpage` etc. in your session.

**`.claude/commands/compare.md`**
```
Take a screenshot of index.html fullpage. Compare it against the last reference image I provided.
List every visual mismatch with px values. Do not fix anything yet.
```

**`.claude/commands/newpage.md`**
```
I want to add a new page called $ARGUMENTS.
Add it to the nav, create its route in the router, and scaffold the section with placeholder content matching the existing design system.
```

**`.claude/commands/tokens.md`**
```
Run /cost and summarise: total tokens this session, biggest single operation, and one suggestion to reduce spend.
```

---

## 6. Figma → Code Workflow (Best Results)

1. In Figma, select the frames you want built and copy the file URL
2. In Claude Code: `"Build this site from my Figma file: [URL]. Start with the Home page."`
3. Claude fetches layout, tokens, components via MCP — no manual colour/spacing extraction
4. After each page: `/compact` then start the next page fresh
5. Use `npx puppeteer-screenshot index.html` to compare rendered output against the Figma frame

**Tip from the community:** Build one page at a time and compact between them. Building all pages in one context window leads to compaction mid-build and lost context.

---

## 7. Tools Worth Installing (Community-Vetted, April 2026)

| Tool | What it does | Install |
|------|-------------|---------|
| `ccusage` | Shows token costs from local session logs — no API needed | `npm install -g ccusage` |
| `claude-token-lens` | Real-time token burn rate per tool/MCP in your session | `npm install -g claude-token-lens` |
| `rtk` (Run Token Killer) | Filters verbose CLI output (npm test, git status) before Claude reads it — can cut 50–90% of command-output tokens | See github.com/rtk-token repo |

---

## 8. What NOT to Do (Common Mistakes)

- **Don't load all MCPs at once** — only enable what the current task needs
- **Don't paste full design docs into CLAUDE.md** — reference them with `@path` instead
- **Don't let sessions run to auto-compact** — use `/compact` manually
- **Don't describe the whole site upfront** — describe one page at a time
- **Don't use screenshots when you have a Figma link** — MCP extraction is more accurate and uses fewer tokens overall
- **Don't ask "how does it look?" in chat** — use the puppeteer screenshot workflow instead; chat questions use context