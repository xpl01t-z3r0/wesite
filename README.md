# React Build Prompt — xpl01t_z3r0 Cybersecurity Portfolio Landing Page

> **How to use this file:**
> Each section below is a self-contained prompt block.
> You can paste the ENTIRE file into Claude/ChatGPT/Copilot for a full build,
> OR paste individual `## SECTION` blocks to rebuild just that component.
> Lines starting with `<!-- CUSTOMIZE:` are manual edit targets — change them before prompting.

---

## GLOBAL CONTEXT (paste this at the top of EVERY prompt)

```
Build a single-page React 18 + Vite cybersecurity portfolio landing page.
Dark HackTheBox-style aesthetic.

Design tokens (already defined in src/assets/style.css, imported globally):
  --color-dark: #111927
  --color-medium: #141d2b
  --color-light: #1a2332
  --color-main: #00efe3        (cyan — primary accent)
  --color-green: #9fef00       (HTB green)
  --color-grey-light: #a4b1cd  (body text)
  --color-red: rgb(255,62,62)
  --color-yellow: rgb(255,175,0)
  --color-blue: rgb(62,177,255)

Font: 'Open Sans' (body), 'Space Mono' (headings/terminals) — both from Google Fonts.

Folder structure:
  src/assets/style.css        (global styles, already written)
  src/assets/particles.json   (particles config)
  src/components/             (one file per section component)
  src/data/                   (writeups.js, skills.js, stats.js — content arrays)
  public/particles.min.js     (particles.js library, loaded via script tag in index.html)
  public/particles.json       (copy, for runtime fetch)

Rules:
- Use CSS variables, never hardcode color hex values.
- Use CSS Modules (*.module.css) per component for scoped styles.
- No Tailwind, no styled-components, no Material UI.
- All user-visible text content comes from src/data/ files — never hardcoded inside JSX.
- Particles.js is initialized via window.particlesJS() after component mounts (useEffect).
- Fully responsive: mobile-first, breakpoint at 650px matches existing style.css media query.
```

---

## SECTION 1 — Project Scaffold

```
Using the GLOBAL CONTEXT above, scaffold the Vite + React 18 project.

Tasks:
1. Output the exact shell commands to create the project:
   npm create vite@latest portfolio -- --template react
   cd portfolio && npm install
   npm install react-router-dom

2. Output index.html with:
   - Google Fonts link: Open Sans, Space Mono
   - <script src="/particles.min.js" defer></script>
   - <div id="particles-js"></div> as a sibling BEFORE <div id="root">
   - Viewport meta tag

3. Output src/main.jsx that imports './assets/style.css' globally.

4. Output src/App.jsx that renders all section components in order:
   <Hero /> <About /> <Skills /> <WriteupFeed /> <ContactFooter />
   No routing needed yet.

5. Output the empty placeholder files (just the export default function shell):
   src/components/Hero.jsx + Hero.module.css
   src/components/About.jsx + About.module.css
   src/components/Skills.jsx + Skills.module.css
   src/components/WriteupFeed.jsx + WriteupFeed.module.css
   src/components/ContactFooter.jsx + ContactFooter.module.css
```

---

## SECTION 2 — Data Files (content lives here — edit these manually)

```
Using the GLOBAL CONTEXT above, generate the three data files.
These files are the ONLY place user-visible content lives.

<!-- CUSTOMIZE: edit the values below before prompting, or edit the output files directly -->

File: src/data/stats.js
Export a default array of stat objects. Initial values:
  { icon: "🏆", value: "42", label: "HTB Machines Pwned" }
  { icon: "🔴", value: "Pro Hacker", label: "HTB Rank" }
  { icon: "🟢", value: "38000+", label: "TryHackMe Points" }
  { icon: "🚩", value: "15+", label: "CTFs Completed" }
  { icon: "💻", value: "20+", label: "GitHub Repos" }

File: src/data/skills.js
Export a default array of skill objects with { name, category }.
Categories map to CSS variable colors:
  "recon"      → var(--color-blue)
  "exploit"    → var(--color-red)
  "forensics"  → var(--color-yellow)
  "lang"       → var(--color-green)
  "tool"       → var(--color-main)

Initial skills:
  Recon: Nmap, Shodan, theHarvester, Maltego, Subfinder, FFUF
  Exploit: Metasploit, SQLmap, Burp Suite, Custom PoCs, Impacket
  Forensics: Ghidra, IDA Free, Volatility, Binwalk, Wireshark, Autopsy
  Lang: Python, Bash, C, PowerShell, Go
  Tool: Docker, Git, tmux, Obsidian, VSCode

File: src/data/writeups.js
Export a default array of writeup objects:
  {
    id: 1,
    title: "Machine Name",
    platform: "HTB",          // "HTB" | "THM" | "CTF"
    difficulty: "Medium",     // "Easy" | "Medium" | "Hard" | "Insane"
    date: "2026-09-10",
    description: "Short one-line description of the challenge or technique.",
    link: "https://github.com/xpl01t-z3r0",
    tags: ["web", "privesc"]
  }
Add 3 placeholder entries. I will replace them with real writeups manually.
```

---

## SECTION 3 — Hero Component

```
Using the GLOBAL CONTEXT above, build src/components/Hero.jsx.

Behaviour:
- On mount, call window.particlesJS('particles-js', config) using a fetch of /particles.json
  inside a useEffect. The #particles-js div is in index.html, not inside this component.
- Show a centered card on top of the particles layer (position: relative, z-index: 1).
- Inside the card, run a typewriter sequence with useEffect + setInterval:
    Line 1: "> whoami" (appears instantly)
    Line 2: types out character by character: "Tofazzel Hossen Topu"
    Line 3: types out: "[ xpl01t_z3r0 ]  |  Offensive Security Enthusiast"
    Line 4: types out: "Bangladesh // HTB // TryHackMe // CTF Player"
  Each line starts typing after the previous one finishes. Cursor blinks after last line.
- Below the typed text, two buttons styled with the .btn class from style.css:
    Primary (filled): "View Writeups" — smooth-scrolls to #writeups section
    Secondary (outline): "Contact" — smooth-scrolls to #contact section

<!-- CUSTOMIZE: change the typewriter lines to match your actual bio -->

Styling in Hero.module.css:
- Card: background var(--color-light), border 1px solid var(--color-main),
  border-radius 8px, padding 40px 48px, max-width 600px, centered.
- Typewriter text: font-family 'Space Mono', color var(--color-main), font-size 14px,
  line-height 2.
- Blinking cursor: ::after pseudo-element, animation blink 1s step-end infinite.
- Hero section height: 100vh, display flex, align-items center, justify-content center.
```

---

## SECTION 4 — About Component

```
Using the GLOBAL CONTEXT above, build src/components/About.jsx.

Import stats from '../data/stats.js'.

Layout (two columns on desktop, stack on mobile):
  Left column: bio text block
  Right column: stat cards grid (2×3 or 3×2)

<!-- CUSTOMIZE: replace the bio paragraph with your own text -->
Bio text (hardcode this one string in the component, or move to data/stats.js if you prefer):
  "Cybersecurity student from Bangladesh with a focus on offensive security.
   I spend my time breaking boxes on HackTheBox, chasing flags in CTFs,
   and building tools that make recon and exploitation faster.
   Currently studying for OSCP."

Stat card: each renders icon (large emoji or Font Awesome icon), value in
var(--color-main) bold large font, label in var(--color-grey-light) small caps.
Card background: var(--color-dark), border 1px solid var(--color-main-dark),
border-radius 6px, padding 20px, text-align center.
On hover: border-color transitions to var(--color-main).

Section id="about", add a section header styled with h2 (font: Space Mono, color: white,
text-transform uppercase, with a left border 4px solid var(--color-main)).
```

---

## SECTION 5 — Skills Component

```
Using the GLOBAL CONTEXT above, build src/components/Skills.jsx.

Import skills from '../data/skills.js'.

Layout:
- Group skills by category. Render one row per category:
    Category label (left, small caps, grey) + tag cloud (right, wrapping flex row)
- Each skill tag: pill shape, border 1px solid <category color>, color <category color>,
  background transparent, border-radius 4px, padding 4px 12px, font-size 12px,
  font-family Space Mono, letter-spacing 1px.
- On hover: background becomes the category color at 10% opacity.

Category → color mapping (use CSS variables):
  recon     → var(--color-blue)
  exploit   → var(--color-red)
  forensics → var(--color-yellow)
  lang      → var(--color-green)
  tool      → var(--color-main)

Section id="skills". Section header same style as About.
```

---

## SECTION 6 — WriteupFeed Component

```
Using the GLOBAL CONTEXT above, build src/components/WriteupFeed.jsx.

Import writeups from '../data/writeups.js'.

Layout: CSS grid, 3 columns on desktop, 2 on tablet (≤900px), 1 on mobile (≤650px).
Each card:
  - Background: var(--color-dark)
  - Border: 1px solid var(--color-main-dark)
  - Border-radius: 6px
  - Padding: 20px
  - On hover: box-shadow 0 0 12px rgba(0,239,227,0.15), border-color var(--color-main)
  - Transition: all 0.2s ease

Card content (top to bottom):
  1. Top row: platform badge + difficulty badge (flex, space-between)
     Platform badge colors:
       HTB  → var(--color-green) border + text
       THM  → var(--color-blue) border + text
       CTF  → var(--color-yellow) border + text
     Difficulty badge colors:
       Easy   → var(--color-green)
       Medium → var(--color-yellow)
       Hard   → var(--color-red)
       Insane → color: #ff00ff (purple)
  2. Title: h3, color white, font Space Mono, margin 12px 0 8px
  3. Date: small, color var(--color-grey-light)
  4. Description: p, color var(--color-grey-light), font-size 13px, line-height 1.6
  5. Tags row: small pill tags, color var(--color-main), border 1px solid var(--color-main-dark)
  6. "[ Read Writeup ]" button at bottom: outline style (.btn.disable class from style.css),
     full width, opens writeup.link in new tab.

Section id="writeups". Section header same style as About.
```

---

## SECTION 7 — ContactFooter Component

```
Using the GLOBAL CONTEXT above, build src/components/ContactFooter.jsx.

<!-- CUSTOMIZE: replace links with your actual profile URLs -->

Social links to render (icon + label + href):
  { icon: "fab fa-github",   label: "GitHub",     href: "https://github.com/xpl01t-z3r0" }
  { icon: "fab fa-linkedin", label: "LinkedIn",   href: "https://linkedin.com/in/tofazzel-hossen-topu-862806424" }
  { icon: "fas fa-cube",     label: "HackTheBox", href: "https://app.hackthebox.com/users/3470445" }
  { icon: "fas fa-flag",     label: "TryHackMe",  href: "https://tryhackme.com/p/Mr.z3ro" }
  { icon: "fas fa-envelope", label: "Email",      href: "mailto:your@email.com" }

Layout:
- Section id="contact", centered content.
- Heading "// get in touch" in Space Mono, var(--color-main).
- Social icons row: Font Awesome icons (already imported in style.css via @import),
  each link is icon + label, color var(--color-grey-light), hover color var(--color-main),
  gap 32px, flex row centered, flex-wrap on mobile.
- Footer bar below: background var(--color-dark), padding 16px,
  text: "xpl01t_z3r0 © 2026 — built with React + particles.js"
  color var(--color-grey-light), font-size 12px, font-family Space Mono, centered.
```

---

## SECTION 8 — Wiring & Final Polish

```
Using the GLOBAL CONTEXT above, do the final wiring pass:

1. Add a fixed top navbar with:
   - Left: "xpl01t_z3r0" in Space Mono + var(--color-main)
   - Right: nav links — About / Skills / Writeups / Contact
     each smooth-scrolls to its section id.
   - Background: var(--color-dark) at 90% opacity, backdrop-filter blur(8px).
   - Border-bottom: 1px solid var(--color-main-dark).
   - On mobile (≤650px): nav links collapse into a hamburger toggle button.

2. Add scroll-behavior: smooth to html {} in style.css.

3. Add section padding: each section gets padding-top 80px (offset for fixed navbar).

4. Add a subtle fade-in animation (opacity 0→1, translateY 20px→0) on each section
   using IntersectionObserver. Trigger once when section enters viewport.

5. Output the final npm scripts to add to package.json:
   "dev": "vite"
   "build": "vite build"
   "deploy": "gh-pages -d dist"
   And: npm install gh-pages --save-dev
   And: vite.config.js with base: '/portfolio/' for GitHub Pages.
```

---

## HOW TO CUSTOMIZE THIS PROMPT FILE

| What you want to change | Where to edit |
|---|---|
| Your name / handle / bio | SECTION 3 typewriter lines + SECTION 4 bio text |
| Stats (HTB rank, machines, etc.) | `src/data/stats.js` directly after build |
| Add a new writeup | Add object to `src/data/writeups.js` |
| Add/remove a skill | Edit `src/data/skills.js` |
| Change color scheme | Edit CSS variables in `src/assets/style.css` |
| Change particle color/speed | Edit `src/assets/particles.json` |
| Add a new section | Write a new SECTION block following the same format, then add component to App.jsx |
| Change social links | SECTION 7 social links array |
| Add a blog/writeups page | Add `react-router-dom` route, create `src/pages/Writeups.jsx` |
