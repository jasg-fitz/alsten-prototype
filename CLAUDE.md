# CLAUDE.md — Alsten Capital Group Prototype

**Project:** High-fidelity animated web prototype — Alsten Capital Group Open Enrollment  
**Owner:** Jasmine Fitzpatrick  
**Status:** Live and deployed. We are now in ongoing iteration.

---

## What this project is

A single-page HTML prototype for a fictional company's benefits enrollment site. It was built from a Figma design file and a written brief. The goal is portfolio-quality: it should look and feel like a real product, not a template.

Jasmine is a design professional learning full-stack web fundamentals through this project. Claude acts as both builder and mentor — explaining decisions, teaching concepts, and doing the work together.

---

## Where everything lives

| Thing | Location |
|-------|----------|
| Live site | `alsten-prototype.jasminefitzpatrick.com` |
| GitHub repo | `github.com/jasg-fitz/alsten-prototype` |
| Cloudflare Pages project | `alsten-prototype` in Cloudflare dashboard |
| Figma source | https://www.figma.com/design/P5QASVmNsiUhU5lf8oNsUl/Alsten-Capital-Group-Case-Study?node-id=11240-3450 |
| Main file | `index.html` — the entire prototype lives here (HTML + CSS + JS in one file) |

---

## How to deploy changes

Every update follows the same 3-step pattern. Claude handles all of it:

```
1. Edit index.html
2. git commit
3. git push → Cloudflare auto-deploys in ~60 seconds
```

Jasmine never needs to touch the Cloudflare dashboard for routine updates. Only for settings changes (custom domain, env vars, etc).

---

## How the code is organised

Everything is in `index.html`. It has three parts:

### 1. CSS (inside `<style>` tags)
Written in sections, top to bottom, matching the page layout:
- **Tokens** — the design variables (colours, fonts, spacing). Change a token and it updates everywhere.
- **Animation system** — reusable classes for scroll reveals and page-load entrances
- **One section per block** — nav, hero, action cards, need help, benefits, app promo, contact, quick links, footer

### 2. HTML (the page content)
Standard semantic structure. Each section is clearly commented. If you're looking for something, search for `══ SECTION NAME ══`.

### 3. JavaScript (inside `<script>` tags at the bottom)
Three things only:
- Page-load entrance animation (nav + hero text)
- The signature hero title clip-reveal
- Scroll-triggered reveals via `IntersectionObserver` — this is what makes elements fade in as you scroll down

---

## Design system (tokens)

These CSS variables live at the top of the `<style>` block. They control the entire visual system:

```css
--black          pure black for hero left panel
--navy           near-black used for most dark sections
--navy-mid       slightly lighter, used for card backgrounds
--navy-card      dark card fill in the benefits grid
--blue           primary brand blue (#2563EB)
--blue-card      blue used inside action cards
--blue-section   dark blue background behind action cards
--orange         accent colour (#F97316)
--white
--gray-text      dimmed white for body copy on dark
--gray-section   medium warm gray (need help + app left bg)
--gray-light     near-white for app right panel bg
--display        Bebas Neue — used only for the ALSTEN logo
--ui             Inter — used for everything else (headings, body, nav)
--max-w          maximum content width (1280px)
--gutter         horizontal padding (80px desktop, scales down)
--radius         card border radius (12px)
```

---

## Page sections (top to bottom)

| Section | Class | Notes |
|---------|-------|-------|
| Nav | `.nav` | Sticky, dark. Logo + links + Enroll button |
| Hero | `.hero` | Split: black left (text), photo right. Signature clip-reveal title animation |
| Action cards | `.action-section` | 3 blue rounded cards: Learn / Explore / Get |
| Need help | `.need-help` | Gray split: photo left, content right |
| Benefits | `.benefits` | Dark navy, 3-column masonry grid of 6 benefit tiles |
| App promo | `.app-promo` | Split: gray left (Tap That App), light right (benefits on the go) |
| Contact | `.contact-band` | Dark, centered, two buttons |
| Quick links | `.quick-links` | Dark, centered, Enroll Now + Check Premiums |
| Footer | `footer` | Centered logo, nav links, copyright |

---

## Animation system

Two types of animation are used:

**Page load (fires immediately on open):**
- Nav items fade up one by one (`data-enter` + `data-delay`)
- Hero title lines clip-reveal upward — this is the signature moment

**Scroll-triggered (fires as you scroll down):**
- Elements with `data-reveal` fade up when they enter the viewport
- `data-reveal-left` and `data-reveal-right` slide in from the sides
- `data-delay` staggers multiple elements in the same section
- All powered by `IntersectionObserver` — no scroll event listeners

Everything is wrapped in `@media (prefers-reduced-motion: reduce)` so it's accessibility-safe.

---

## Figma source

- **Access:** Connect via MCP to Jasmine's Figma account (use `/login` at session start if needed)
- **Node to load:** `node-id=11240-3450` (the full prototype page)
- **Important pages in the file:** Wireframe (5999:10563), Style Guide (2368:52)
- The Figma is the design source of truth. If something looks wrong, check Figma before assuming.

---

## Design direction

- Pull all visual decisions from Figma. Do not invent a new visual system.
- Where Figma is ambiguous, make decisions consistent with what's already established, then log it below.
- Font stack: **Inter** (all headings and body), **Bebas Neue** (logo only)
- Desktop-first. Primary viewport: 1280px–1440px. Must not break at 768px.
- Images: Currently using Unsplash placeholder URLs. Replace with real assets when provided.

---

## Animation principles

- Page load: orchestrated entrance sequence, not simultaneous fades
- Scroll reveals: `IntersectionObserver` only, never scroll listeners
- Hover: every interactive element responds visibly
- Timing: 150–300ms micro-interactions, up to 650ms for section reveals
- Easing: `cubic-bezier(0.16, 1, 0.3, 1)` throughout — feels physical
- One signature moment (hero title clip-reveal) — everything else supports it

---

## Known issues / things still to fix

- [ ] Hero right-panel photo is a generic Unsplash image — Figma shows a branded lifestyle shot (tablet + Alsten water bottle). Needs a closer match or real asset.
- [ ] Action cards say "What's new for 2026" — confirm correct year with brief
- [ ] Need Help section: stock photo placeholder, should be a real people/decision image
- [ ] App promo left panel: uses an SVG placeholder icon instead of a real app screenshot
- [ ] Benefits grid: Dental Care + Tax-Advantaged tiles use stock photos — could be stronger
- [ ] Typography: section headings may not exactly match Figma weight/tracking — needs visual QA against each section
- [ ] No mobile nav (hamburger menu) — currently nav links just disappear at 768px
- [ ] Hover states not verified on all interactive elements
- [ ] Fonts are loaded from Google Fonts CDN — if presenting offline, they'll fall back to system fonts

---

## Key decisions log

| Decision | Reason | Date |
|----------|--------|------|
| Single `index.html` file (no separate CSS/JS) | Simpler deployment, easier to share, no build step | Jun 2026 |
| Inter 900 for section headings (not Bebas Neue) | Figma designs use a heavy proportional sans-serif, not the condensed display look of Bebas Neue | Jun 2026 |
| Bebas Neue kept for logo wordmark only | Logo's ALSTEN treatment specifically matches Bebas Neue proportions | Jun 2026 |
| Unsplash URLs for all photos | No real assets provided yet; placeholder URLs are fast and reliable | Jun 2026 |
| Subdomain `alsten-prototype.jasminefitzpatrick.com` (not a path) | Cloudflare Pages deploys at domain/subdomain level — a path would require Workers routing | Jun 2026 |
| `.gitignore` excludes `.png` files and `strata-capital.html` | Reference images and other prototypes don't need to be in the deployment repo | Jun 2026 |

---

## Session workflow (for each new Claude Code session)

1. **Read this file first** — get oriented before touching anything
2. **Read `index.html`** — understand the current state before editing
3. **Check Figma** — if fixing a visual issue, screenshot the relevant node before changing code
4. **Edit, commit, push** — one logical change per commit, descriptive message
5. **Update this file** — log new decisions, tick off fixed issues, add new known issues

---

## How Jasmine likes to work

- Explain what you're doing and why, as you go — this is a learning project
- When introducing a new concept (CSS property, git command, etc.), give a one-line plain-English explanation
- Don't over-engineer. Simple, readable code over clever abstractions.
- Flag anything that looks wrong visually, even if it wasn't in scope
