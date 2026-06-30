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
| Figma source | https://www.figma.com/design/P5QASVmNsiUhU5lf8oNsUl/Alsten-Capital-Group-Case-Study (Home • Desktop frame: `node-id=11202-68859`) |
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

**Always preview locally and wait for Jasmine's approval before committing/pushing.** A local
static server config lives in `.claude/launch.json` (`python3 -m http.server 3000`) for the preview panel.

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
Four things only:
- Page-load entrance animation (nav + hero text)
- The signature hero title clip-reveal
- Scroll-triggered reveals via `IntersectionObserver` — this is what makes elements fade in as you scroll down
- Mobile nav: the burger button toggles `.open` on `.nav` to show/hide the links dropdown

---

## Design system (tokens)

These CSS variables live at the top of the `<style>` block. They control the entire visual system:

```css
--black          near-black #030308 — nav, hero left panel, contact band, footer
--navy           near-black used for most dark sections (benefits, need help content)
--navy-mid       slightly lighter — quick links background
--navy-card      dark card fill in the benefits grid
--blue           primary brand blue (#2563EB) — all buttons + CTAs
--blue-card      blue used inside action cards
--blue-section   dark blue background behind action cards (#1A3CBD)
--blue-deep      deep saturated blue for the app promo section (#0E2766)
--orange         accent colour (#F97316)
--white
--gray-text      dimmed white for body copy on dark
--rule-dark      hairline borders/dividers on dark
--rule-light     hairline borders on light
--display        Barlow Condensed — all headings (700/800, uppercase)
--ui             Barlow — body copy, nav links, buttons, taglines
--max-w          maximum content width (1280px)
--gutter         horizontal padding (80px desktop → 40px → 24px mobile)
--radius         card border radius (12px)
```

**Type system:** Barlow Condensed (headings + logo wordmark fallback), Barlow (everything else),
Inter (hero date line only — Semi Bold 16px). The logo is a real SVG, not type.
The "Is Here!" word is true italic — the italic Barlow Condensed faces are loaded so it's a
designed italic, not a faux browser slant. Hero body + date use the warm grey `#B6B4B3` at 150% line-height.

**Chevron icons:** all "›" CTA arrows (action cards, benefit tiles, quick-links buttons) are the
`chevron_right.svg` rendered via CSS `mask` on a `.icon-chevron` element, so each chevron inherits
its text colour (and hover colour). Sized in `em`: action-card arrows are `1em`, benefit + quick-links
arrows are `2em/3`. They nudge right on hover.

---

## Page sections (top to bottom)

| Section | Class | Notes |
|---------|-------|-------|
| Nav | `.nav` | Sticky, black. SVG logo left; links right next to Enroll button. Burger menu < 768px |
| Hero | `.hero` | Split: black left (text), photo right (`Header-image.jpg`). Signature clip-reveal title. Live pulsing dot on the tag |
| Action cards | `.action-section` | 3 blue rounded cards (Learn / Explore / Get), 32px gap, that float up and overlap the hero bottom |
| Need help | `.need-help` | Full-bleed photo with diagonal dark gradient; content block right-aligned over it |
| Benefits | `.benefits` | Dark navy, 3-column masonry grid of 6 tiles. Each tile: 40px padding; description + right-aligned chevron CTA (no pill) |
| App promo | `.app-promo` | Deep-blue (`--blue-deep`); phone mockup left (flush to bottom edge), content right |
| Contact | `.contact-band` | Black, centered, two blue buttons |
| Quick links | `.quick-links` | Dark, centered: Enroll Now + Check Premiums + Contacts |
| Footer | `footer` | Black. Logo left, nav links right; divider; copyright + legal links |

**Buttons:** all CTAs are 16px and use the brand blue (`--blue`). On mobile every button goes full width.

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
- **Node to load:** `node-id=11202-68859` — the finished **Home • Desktop** frame
- **Mobile frame:** also in the file; `Home • Mobile.png` export lives locally for reference
- The Figma is the design source of truth. If something looks wrong, check Figma before assuming.

---

## Design direction

- Pull all visual decisions from Figma. Do not invent a new visual system.
- Where Figma is ambiguous, make decisions consistent with what's already established, then log it below.
- Font stack: **Barlow Condensed** (headings), **Barlow** (body/nav/buttons), **Inter** (hero date only). Logo is an SVG.
- Desktop-first. Primary viewport: 1280px–1440px. Fully optimised for mobile (≤ 768px) — see below.
- Images: real assets now live in `assets/` (Images, Icons, Logos). No more Unsplash placeholders.

---

## Mobile (≤ 768px)

The whole page is optimised for mobile, designed against `Home • Mobile.png` (local reference, gitignored):
- **Nav** — logo left, Enroll + hamburger right. Burger opens a dropdown panel with the links.
- **Buttons** — all CTAs go full width.
- **Benefits** — single column. Tiles use `--m-order` (set inline per tile) so the mobile order is
  Medical → Prescription → Dental → Vision → Tax-Advantaged → Life, independent of the desktop column layout.
- **App promo** — `column-reverse` so the order becomes heading → body → Get The App → "Tap That App!" → phone.
- **Footer** — content horizontally centered.

---

## Assets (in `assets/`)

- **Logos/** — `Alsten Capital Group Logo - Light.svg` (used in nav + footer), `… - Dark.svg`
- **Images/** — `Header-image.jpg` (hero), `App mockup.png` (app promo phone),
  `Benefits Image - Medical.jpg`, `Benefits Image - Dentist.jpg`, `Benefits Image - Tax Accounts.jpg`,
  `vitaly-gariev-8k5j5z6ZYT4-unsplash.jpg` (need help)
- **Icons/** — `medication.svg` (prescription), `visibility.svg` (vision), `shield_person.svg` (life),
  `chevron_right.svg` (all CTA arrows), `arrow_forward.svg` (available, currently unused)
- These asset files ARE committed. Reference/working files (figma-*.png, wireframe, mobile mockup, phone-mockup.svg) are gitignored.

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

- [x] ~~Hero photo / Need Help / app screenshot / benefit images~~ — all replaced with real branded assets
- [x] ~~No mobile nav~~ — hamburger menu added; full mobile pass complete
- [x] ~~Typography QA~~ — Barlow Condensed/Barlow/Inter system applied throughout
- [x] ~~Hero text alignment~~ — hero text now lines up with the nav/footer/benefits container; the photo stays full-bleed
- [ ] Hover states not exhaustively verified on every interactive element
- [ ] Fonts load from Google Fonts CDN — if presenting offline they'll fall back to system fonts

---

## Key decisions log

| Decision | Reason | Date |
|----------|--------|------|
| Single `index.html` file (no separate CSS/JS) | Simpler deployment, easier to share, no build step | Jun 2026 |
| Type system: Barlow Condensed (headings) + Barlow (body/nav/buttons) + Inter (hero date) | Shows intentional typeface pairing for a design case study; replaced earlier Inter/Bebas Neue exploration | Jun 2026 |
| Logo is a real SVG (not type) | Brand wordmark + mark supplied as `Alsten Capital Group Logo - Light.svg` | Jun 2026 |
| Real assets committed under `assets/` | Replaced all Unsplash placeholders once branded exports were provided | Jun 2026 |
| All CTAs 16px brand-blue; full-width on mobile | Consistency across the page per Figma | Jun 2026 |
| Need Help is a full-bleed photo + gradient (not a split) | Matches the finished Figma design | Jun 2026 |
| App promo phone sits flush to the section's bottom edge | Matches Figma; achieved by zeroing the section's bottom padding | Jun 2026 |
| Benefits mobile order via inline `--m-order` + `display:contents` on columns | Lets the 3-column desktop layout reflow into the correct single-column mobile order | Jun 2026 |
| Nav gutter padding moved outside the max-width container | So nav content aligns with footer/benefits/contact instead of being inset by an extra gutter | Jun 2026 |
| Hero text left padding = `max(--gutter, calc(100% - --max-w / 2))` | Lines the hero text up with the centered container while the photo stays full-bleed; the `100%` (column width) avoids the scrollbar offset that `100vw` causes | Jun 2026 |
| CTA arrows are an SVG mask (`chevron_right.svg`), sized in `em` | Inherit text colour + hover state, scale with adjacent text; replaced the text "›" glyph and the bordered "Explore" pill on benefit tiles | Jun 2026 |
| `--black` is `#030308`, not pure `#000000` | Slightly warmer near-black reads better against the hero photo and blue sections | Jun 2026 |
| Subdomain `alsten-prototype.jasminefitzpatrick.com` (not a path) | Cloudflare Pages deploys at domain/subdomain level — a path would require Workers routing | Jun 2026 |
| `.gitignore` excludes reference screenshots (figma-*, wireframe, mobile mockup, phone-mockup.svg) + `strata-capital.html` | Working references don't belong in the deployment repo; the `assets/` files ARE tracked | Jun 2026 |

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
