# SaboTrack — Showcase Website Design Spec

**Date:** 2026-06-23  
**Stack:** Single HTML file + Tailwind CSS via CDN  
**Hosting:** GitHub Pages  
**Languages:** English (default), Italian (toggle)

---

## Overview

A single-page showcase website for **SaboTrack**, a party racing game for Apple TV (host) and iPhone (controller). The site presents the game to players, press, and the Apple Foundation Program audience. It is mobile-first, with an Apple-inspired minimal aesthetic.

---

## Visual Design

| Token | Value |
|---|---|
| Background primary | `#FFFFFF` |
| Background secondary | `#F5F5F7` |
| Background dark | `#1D1D1F` |
| Accent | To be confirmed from `icon.png` at build time |
| Font — headings | Audiowide (Google Fonts) |
| Font — body | SF Pro (system font stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`) |

**Principles:** generous whitespace, rounded corners (`rounded-2xl`), subtle shadows, colors used as accents only (not backgrounds). Fully responsive — mobile-first breakpoints with `md:` and `lg:` Tailwind prefixes.

---

## i18n

Language is toggled client-side via vanilla JS. All user-facing strings are stored in a `translations` object with `en` and `it` keys. On toggle, JS iterates all `[data-i18n]` elements and swaps `textContent`. Default language: English. The toggle is a pill switcher in the navbar.

---

## Page Sections

### Navbar (sticky)
- Left: `icon.png` + "SaboTrack" wordmark in Audiowide
- Right: EN / IT pill toggle
- White background; `border-b` appears on scroll via JS scroll listener
- Z-index above all sections

### 1. Hero
- Background: `#F5F5F7`
- Layout: text left, image right on `lg:`, stacked (text top, image bottom) on mobile
- Content:
  - Headline: "SaboTrack" in Audiowide, large (`text-5xl md:text-7xl`)
  - Tagline (i18n): *"The track changes. The chaos is yours."*
  - Platform badges: "Apple TV" + "iPhone" as inline chips
  - CTA buttons: **Watch Trailer** (anchor scroll to `#trailer`) + **Learn More** (anchor scroll to `#game`)
- Hero image: `SaboTrackHD.png`

### 2. Trailer
- Background: `#1D1D1F` (dark section)
- Section title (i18n): "Watch the Trailer"
- YouTube `<iframe>` embed with placeholder URL `https://www.youtube.com/embed/dQw4w9WgXcQ`
- Responsive via `aspect-video` Tailwind class, `max-w-4xl`, centered, `rounded-2xl`

### 3. Game Description — Design Pillars
- Background: `#FFFFFF`
- Section title (i18n): "How It Works"
- Intro paragraph: short summary of the core loop (i18n)
- Three cards in column on mobile, `grid-cols-3` on `lg:`:
  - **Controlled Chaos** — image: `Controller Chaos.png`, subtitle + body (i18n)
  - **Comeback Loop** — image: `Comeback Loop.png`, subtitle + body (i18n)
  - **Split Experience** — image: `Splite Experience.png`, subtitle + body (i18n)
- Each card: image top (rounded), Audiowide title, body text, `shadow-sm`, `rounded-2xl`

### 4. Screenshots
- Background: `#F5F5F7`
- Section title (i18n): "Screenshots"
- CSS grid: `grid-cols-2` on mobile, `grid-cols-4` on `lg:`
- Images: `ScreenshotPista.png`, `ScreenshotTV.png`, `ScreenshotMobile1.png`, `ScreenshotMobile2.png`, `racetime.png`, `sabotime.png`
- Each image: `rounded-2xl`, `object-cover`, `w-full`, tap opens lightbox (simple CSS/JS overlay)

### 5. The Team
- Background: `#FFFFFF`
- Section title (i18n): "Meet the Team"
- Grid: `grid-cols-2` on mobile, `grid-cols-4` on `lg:`
- Four cards, one per member:
  | Name | Degree | Roles |
  |---|---|---|
  | Federico Agnello | BSc Computer Science | Game Designer, Developer, Video Editor, 3D Modeller |
  | Marco Bellante | BSc Computer Science | UI/UX Designer, Graphic Designer, Logo Designer |
  | Dario Giuseppe Maria Rizzo | MSc Computer Science | Game Designer, Developer, UI/UX Designer, 3D Environment |
  | Paolosalvatore Piazza | BSc Computer Engineering | UI/UX Designer, Developer, Logo Designer, Graphic Designer |
- Each card: name in Audiowide, degree in small gray text, roles as colored chips (`bg-accent/10 text-accent`)
- No individual photos (no individual photo assets available)

### 6. Our Story — AFP
- Background: `#F5F5F7`
- Section title (i18n): "Our Story"
- Layout: two columns on `lg:` (text left, photo right), stacked on mobile
- Text (i18n): why the team built SaboTrack + description of the Apple Foundation Program Avanzato 2026 experience
- Photo: `afp_avanzato_2026.JPG`, `rounded-2xl`, full-width in its column

### 7. Footer
- Background: `#1D1D1F`, white text
- Content: "SaboTrack" wordmark, "© 2026", optional GitHub repo link
- Minimal — single row on desktop, stacked on mobile

---

## File Structure

```
/
├── index.html          # Single file — all HTML, inline <script>, links to Tailwind CDN
├── icon.png
├── SaboTrackHD.png
├── mockup1.png / mockup2.png / mockup3.png
├── Controller Chaos.png
├── Comeback Loop.png
├── Splite Experience.png
├── ScreenshotPista.png
├── ScreenshotTV.png
├── ScreenshotMobile1.png
├── ScreenshotMobile2.png
├── racetime.png
├── sabotime.png
├── team.png
└── afp_avanzato_2026.JPG
```

---

## JavaScript Requirements

- **i18n toggle:** swap `data-i18n` element text on EN/IT click; persist choice to `localStorage`
- **Navbar border:** add `border-b border-gray-200` on `window.scroll > 0`
- **Lightbox:** click on screenshot → full-screen overlay with the image; click/tap to close

---

## GitHub Pages Deployment

No build step required. Push `index.html` and all assets to the `main` branch (or a `gh-pages` branch). Enable GitHub Pages in repo settings pointing to the appropriate branch/root. The site is live at `https://<username>.github.io/<repo>/`.
