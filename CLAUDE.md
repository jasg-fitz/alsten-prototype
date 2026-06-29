# CLAUDE.md — High-Fidelity Web Prototype

**Project:** Figma-to-HTML high-fidelity prototype  
**Owner:** Jasmine Fitzpatrick  
**Goal:** Take a wireframe brief and Figma designs and produce a polished, highly animated production-quality HTML/CSS/JS prototype, and guide me through deploying it to Cloudflare pages (jasminefitzpatrick.com), through git.

---

## Project overview

This project converts a design brief (an email + wireframe) and Figma source designs into a high-fidelity, animated HTML prototype. The output is a single HTML file (or modular HTML/CSS/JS folder) that closely mirrors the Figma designs, brings motion to life, and is ready to present as a portfolio-quality deliverable.

The workflow:
1. Brief (email + wireframe) defines scope, content, and intent
2. Existing HTML prototype captures early thinking, preserve what works
3. Figma designs (accessed post-login) are the design source of truth
4. Final output is a high-fidelity, animated website prototype

---

## Source materials

### Brief
In folder

### Existing HTML prototype
In folder

### Figma designs
- **URL:** https://www.figma.com/design/P5QASVmNsiUhU5lf8oNsUl/Alsten-Capital-Group-Case-Study?node-id=11240-3450
- **Access method:** Connect via MCP to my Figma account

---

## Design direction

Pull all visual decisions from the Figma designs. Do not invent a new visual system. If the Figma files contain:

- Color tokens or styles: use them exactly
- Type scales or text styles: match them
- Component states (hover, active, etc.): implement them all
- Spacing/grid: replicate the layout logic

Where Figma designs are ambiguous or incomplete, make decisions consistent with the visual system already established, then flag them as assumptions in comments.

---

## Animation principles

Bring the prototype to life with deliberate, purposeful motion. Specifically:

- **Page load:** orchestrate an entrance sequence, not a dump of simultaneous fades
- **Scroll-triggered reveals:** use IntersectionObserver, not scroll event listeners
- **Hover micro-interactions:** every interactive element should respond visibly to hover/focus
- **Transitions:** keep them fast (150–300ms for micro, up to 600ms for section reveals)
- **Easing:** use `cubic-bezier` curves that feel physical, not `ease` defaults
- **Reduced motion:** wrap all animations in `@media (prefers-reduced-motion: reduce)` and provide a no-animation fallback

Prioritise one signature animation moment that makes the prototype feel high-end. Everything else should support it, not compete with it.

---

## Technical constraints

- **Output format:** Single HTML file preferred (self-contained), or `/index.html` + `/style.css` + `/script.js` if complexity requires it
- **No frameworks:** Vanilla HTML, CSS, and JavaScript only (no React, no Vue, no build tools)
- **No CDN dependencies** unless pre-approved (avoid loading from external URLs that might be blocked in presentation)
- **Fonts:** Use Google Fonts `@import` at the top of CSS, or embed as base64 if needed for offline use
- **Images:** Use placeholder URLs initially (`https://picsum.photos` or inline SVG), replace with real assets when provided
- **Browser target:** Chrome (latest) — this is a prototype, not a production cross-browser build
- **Responsive:** Desktop-first, but must not break catastrophically on a laptop viewport (1280px–1440px is the primary target)

---

## Code quality standards

- Semantic HTML elements (`<section>`, `<nav>`, `<article>`, `<header>`, `<footer>`)
- CSS custom properties (`--color-primary`, `--space-lg`) for all tokens
- BEM-style class naming or clear component-scoped naming
- No inline styles except for dynamic JS-driven values
- Comments on every major section and any non-obvious CSS trick
- JS: use `const`/`let`, no `var`; functions named by what they do, not what they are

---

## Workflow for each Claude Code session

1. **Review source materials first** — read the brief, skim the existing prototype, note gaps
2. **Read Figma designs** — after login, systematically extract: colors, type, spacing, components, states
3. **Plan before coding** — list the sections/components to build, in order
4. **Build section by section** — complete one section fully (HTML + CSS + JS + animation) before moving to the next
5. **Screenshot and critique** — if your environment allows, take a screenshot after each section and compare to Figma
6. **Animate last pass** — once structure and styles are solid, layer in scroll and load animations
7. **Final review** — check spacing consistency, type hierarchy, hover states, and reduced-motion behavior

---

## Key decisions log

Use this section to record choices made during the build. Add an entry any time you make a decision that isn't directly dictated by the Figma or brief.

| Decision | Reason | Date |
|----------|--------|------|
| [e.g. Used SVG inline for logo] | [e.g. Avoids asset dependency] | [date] |

---

## Known assumptions

List anything assumed in the absence of explicit direction:

- [ ] [e.g. Mobile breakpoint behavior is not specified in Figma — assuming stack to single column at 768px]
- [ ] [add more as they arise]

---

## Files in this project

```
/
├── CLAUDE.md          ← this file
├── index.html         ← main prototype output
├── style.css          ← extracted if needed for readability
├── script.js          ← extracted if needed for readability
├── assets/            ← any provided images or icons
└── reference/         ← screenshots from Figma or brief (if saved locally)
```

---

## How to use this file

This file lives at the root of the project. At the start of every Claude Code session, Claude should read it first to orient to the project context, design direction, and what has already been built. Update the **Key decisions log** and **Known assumptions** sections as the project progresses.

If Jasmine provides new source materials (email, wireframe images, Figma URL), add them to the **Source materials** section immediately.
