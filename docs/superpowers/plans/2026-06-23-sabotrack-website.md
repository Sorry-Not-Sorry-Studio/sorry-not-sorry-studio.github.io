# SaboTrack Showcase Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page bilingual (EN/IT) showcase website for SaboTrack, hosted on GitHub Pages.

**Architecture:** One `index.html` file at the project root. Tailwind CSS loaded via CDN. Vanilla JS handles i18n (translations object + `data-i18n` attributes), navbar scroll behavior, and a lightbox. No build step.

**Tech Stack:** HTML5, Tailwind CSS v3 (Play CDN), Vanilla JS, Google Fonts (Audiowide), GitHub Pages.

## Global Constraints

- Single file: `index.html` at project root — no build, no node_modules
- Tailwind: `<script src="https://cdn.tailwindcss.com"></script>` then `tailwind.config = {...}` in a following `<script>` block
- Heading font: Audiowide via Google Fonts — class `font-display` (defined in tailwind config)
- Body font: system SF Pro stack — `font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`
- Accent color: `#FF5C00` — configured as `accent` in tailwind theme
- Backgrounds: `#FFFFFF` (primary), `#F5F5F7` (secondary), `#1D1D1F` (dark)
- Text: `#1D1D1F` (primary), `#6E6E73` (secondary/muted)
- Default language: English — saved to `localStorage` key `sabotrack-lang`
- Image filenames with spaces **must** be URL-encoded in `src` attributes: `Controller%20Chaos.png`, `Comeback%20Loop.png`, `Splite%20Experience.png`
- All assets already present in the project root — no downloads needed
- Mobile-first; breakpoints `md:` (768px) and `lg:` (1024px)
- Rounded corners: `rounded-2xl` throughout
- No external JS libraries

---

### Task 1: HTML scaffold + Navbar + i18n system + JS utilities

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: `applyLang(lang: 'en' | 'it'): void` — called by navbar toggle buttons and on DOMContentLoaded
- Produces: `openLightbox(src: string, alt: string): void` — called by screenshot `onclick`
- Produces: `closeLightbox(): void`
- Produces: `translations` object with all EN/IT string keys — later tasks add `data-i18n` attributes matching these keys
- Produces: `[data-i18n="key"]` convention — every i18n string in the HTML uses this attribute

- [ ] **Step 1: Create `index.html` with head, Tailwind config, navbar, lightbox overlay, and all JS**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SaboTrack</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: { accent: '#FF5C00' },
          fontFamily: {
            display: ['Audiowide', 'sans-serif'],
          },
        }
      }
    }
  </script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Audiowide&display=swap" rel="stylesheet">
  <style>
    html { scroll-behavior: smooth; }
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif; }
    .chip { background-color: rgba(255,92,0,0.1); color: #FF5C00; }
  </style>
</head>
<body class="bg-white text-[#1D1D1F] antialiased">

  <!-- NAVBAR -->
  <nav id="navbar" class="fixed top-0 left-0 right-0 z-40 bg-white transition-all duration-200">
    <div class="max-w-6xl mx-auto px-6 h-16 flex items-center justify-between">
      <a href="#" class="flex items-center gap-3">
        <img src="icon.png" alt="SaboTrack icon" class="h-8 w-8 rounded-lg">
        <span class="font-display text-xl text-[#1D1D1F]">SaboTrack</span>
      </a>
      <div class="flex items-center gap-1 bg-[#F5F5F7] rounded-full p-1">
        <button data-lang="en" class="lang-btn px-3 py-1 rounded-full text-sm font-medium transition-all duration-200 bg-white shadow text-[#1D1D1F]" onclick="applyLang('en')">EN</button>
        <button data-lang="it" class="lang-btn px-3 py-1 rounded-full text-sm font-medium transition-all duration-200 text-[#6E6E73]" onclick="applyLang('it')">IT</button>
      </div>
    </div>
  </nav>

  <!-- MAIN CONTENT PLACEHOLDER -->
  <main class="pt-16"></main>

  <!-- LIGHTBOX -->
  <div id="lightbox" class="fixed inset-0 bg-black/90 z-50 hidden items-center justify-center p-4" role="dialog" aria-modal="true">
    <button id="lightbox-close" class="absolute top-4 right-4 text-white text-4xl leading-none" aria-label="Close">&times;</button>
    <img id="lightbox-img" src="" alt="" class="max-h-[90vh] max-w-full rounded-2xl object-contain">
  </div>

  <script>
    // ── i18n ──────────────────────────────────────────────────────────────────
    const translations = {
      en: {
        'hero.tagline': 'The track changes. The chaos is yours.',
        'hero.cta.trailer': 'Watch Trailer',
        'hero.cta.learn': 'Learn More',
        'trailer.title': 'Watch the Trailer',
        'game.title': 'How It Works',
        'game.intro': 'Each race, players place objects on the track — ramps, oil slicks, spinning hammers — shaping the chaos for everyone. Score points by finishing races, triggering your own objects, and outsmarting your rivals.',
        'game.pillar1.title': 'Controlled Chaos',
        'game.pillar1.body': 'Every race is unpredictable, but the unpredictability always comes from deliberate player choices — not pure randomness. The track transforms lap by lap by collective will.',
        'game.pillar2.title': 'Comeback Loop',
        'game.pillar2.body': 'Players behind in the standings always have concrete tools to catch up. No one is out of the game before the last race.',
        'game.pillar3.title': 'Split Experience',
        'game.pillar3.body': 'TV and phone are two complementary roles in the same experience. The shared screen creates social moments; the phone creates intimacy and control.',
        'screenshots.title': 'Screenshots',
        'team.title': 'Meet the Team',
        'story.title': 'Our Story',
        'story.why.heading': 'Why We Built SaboTrack',
        'story.why.body': 'We wanted to create a game that feels like a party from the moment you turn it on — chaotic, social, and always fair. SaboTrack was born from our love of party games and our frustration with games that leave players feeling powerless.',
        'story.afp.heading': 'Apple Foundation Program',
        'story.afp.body': 'SaboTrack was developed as part of the Apple Foundation Program Avanzato 2026, an intensive program where selected developers build real products using Apple technologies. The experience shaped every design decision — from the asymmetric Apple TV + iPhone setup to the clean, minimal aesthetic.',
        'footer.rights': '© 2026 SaboTrack. All rights reserved.',
      },
      it: {
        'hero.tagline': 'Il tracciato cambia. Il caos è tuo.',
        'hero.cta.trailer': 'Guarda il Trailer',
        'hero.cta.learn': 'Scopri di più',
        'trailer.title': 'Guarda il Trailer',
        'game.title': 'Come Funziona',
        'game.intro': "Ad ogni gara, i giocatori piazzano oggetti sulla pista — rampe, pozze d'olio, martelli rotanti — plasmando il caos per tutti. Accumula punti finendo le gare, attivando i tuoi oggetti e sorprendendo i tuoi avversari.",
        'game.pillar1.title': 'Caos Controllato',
        'game.pillar1.body': "Ogni corsa è imprevedibile, ma l'imprevedibilità nasce sempre da decisioni deliberate dei giocatori, non da casualità pura. Il tracciato si trasforma giro dopo giro per volere collettivo.",
        'game.pillar2.title': 'Rivalsa Possibile',
        'game.pillar2.body': "Chi è indietro in classifica ha sempre strumenti concreti per rimontare. Nessun giocatore si sente fuori dalla partita prima dell'ultima corsa.",
        'game.pillar3.title': 'Split Experience',
        'game.pillar3.body': "TV e telefono non sono due giochi separati: sono due ruoli complementari della stessa esperienza. Lo schermo condiviso crea momenti sociali; il telefono crea intimità e controllo.",
        'screenshots.title': 'Screenshot',
        'team.title': 'Il Team',
        'story.title': 'La Nostra Storia',
        'story.why.heading': 'Perché abbiamo creato SaboTrack',
        'story.why.body': 'Volevamo creare un gioco che si senta come una festa dal momento in cui lo accendi — caotico, sociale e sempre equo. SaboTrack è nato dal nostro amore per i party game e dalla frustrazione per i giochi che lasciano i giocatori senza strumenti per rimontare.',
        'story.afp.heading': 'Apple Foundation Program',
        'story.afp.body': "SaboTrack è stato sviluppato nell'ambito dell'Apple Foundation Program Avanzato 2026, un programma intensivo in cui sviluppatori selezionati costruiscono prodotti reali usando le tecnologie Apple. L'esperienza ha plasmato ogni decisione di design — dalla configurazione asimmetrica Apple TV + iPhone all'estetica pulita e minimale.",
        'footer.rights': '© 2026 SaboTrack. Tutti i diritti riservati.',
      },
    };

    let currentLang = localStorage.getItem('sabotrack-lang') || 'en';

    function applyLang(lang) {
      currentLang = lang;
      localStorage.setItem('sabotrack-lang', lang);
      document.documentElement.lang = lang;
      document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.dataset.i18n;
        if (translations[lang][key] !== undefined) el.textContent = translations[lang][key];
      });
      document.querySelectorAll('.lang-btn').forEach(btn => {
        const active = btn.dataset.lang === lang;
        btn.classList.toggle('bg-white', active);
        btn.classList.toggle('shadow', active);
        btn.classList.toggle('text-[#1D1D1F]', active);
        btn.classList.toggle('text-[#6E6E73]', !active);
      });
    }

    // ── Navbar scroll border ──────────────────────────────────────────────────
    window.addEventListener('scroll', () => {
      const nav = document.getElementById('navbar');
      nav.classList.toggle('border-b', window.scrollY > 0);
      nav.classList.toggle('border-gray-200', window.scrollY > 0);
    }, { passive: true });

    // ── Lightbox ──────────────────────────────────────────────────────────────
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');

    function openLightbox(src, alt) {
      lightboxImg.src = src;
      lightboxImg.alt = alt;
      lightbox.classList.remove('hidden');
      lightbox.classList.add('flex');
      document.body.style.overflow = 'hidden';
    }

    function closeLightbox() {
      lightbox.classList.add('hidden');
      lightbox.classList.remove('flex');
      lightboxImg.src = '';
      document.body.style.overflow = '';
    }

    document.getElementById('lightbox-close').addEventListener('click', closeLightbox);
    lightbox.addEventListener('click', e => { if (e.target === lightbox) closeLightbox(); });
    document.addEventListener('keydown', e => { if (e.key === 'Escape') closeLightbox(); });

    // ── Init ──────────────────────────────────────────────────────────────────
    document.addEventListener('DOMContentLoaded', () => applyLang(currentLang));
  </script>
</body>
</html>
```

- [ ] **Step 2: Open `index.html` in browser and verify**

Open the file directly (double-click or `File > Open`). Expected:
- Navbar visible at top: `icon.png` + "SaboTrack" wordmark on left, EN/IT toggle on right
- EN button has white background + shadow; IT button has gray text
- Clicking IT swaps the active styling; refreshing keeps IT selected (localStorage)
- Scrolling adds a bottom border to the navbar (test by adding `<div style="height:200vh"></div>` temporarily inside `<main>`)
- Audiowide font loads for the "SaboTrack" wordmark (distinctive rounded geometric letters)

- [ ] **Step 3: Initialize git and commit**

```bash
git init
git add index.html
git commit -m "feat: scaffold — navbar, i18n system, lightbox JS"
```

---

### Task 2: Hero section

**Files:**
- Modify: `index.html` — replace `<!-- MAIN CONTENT PLACEHOLDER -->\n  <main class="pt-16"></main>` with the full hero section

**Interfaces:**
- Consumes: `translations` keys `hero.tagline`, `hero.cta.trailer`, `hero.cta.learn`
- Produces: `id="hero"` section; `#trailer` and `#game` are anchor targets added in Tasks 3 and 4

- [ ] **Step 1: Replace the `<main>` placeholder**

Find this in `index.html`:
```html
  <!-- MAIN CONTENT PLACEHOLDER -->
  <main class="pt-16"></main>
```

Replace with:
```html
  <main class="pt-16">

    <!-- ── HERO ──────────────────────────────────────────────────────────── -->
    <section id="hero" class="bg-[#F5F5F7] min-h-[90vh] flex items-center">
      <div class="max-w-6xl mx-auto px-6 py-20 w-full">
        <div class="flex flex-col lg:flex-row items-center gap-12">
          <div class="flex-1 text-center lg:text-left">
            <h1 class="font-display text-6xl md:text-7xl lg:text-8xl text-[#1D1D1F] mb-4">SaboTrack</h1>
            <p class="text-xl md:text-2xl text-[#6E6E73] mb-8 max-w-lg mx-auto lg:mx-0" data-i18n="hero.tagline">The track changes. The chaos is yours.</p>
            <div class="flex flex-wrap gap-2 justify-center lg:justify-start mb-8">
              <span class="px-3 py-1 rounded-full bg-[#1D1D1F] text-white text-sm font-medium">Apple TV</span>
              <span class="px-3 py-1 rounded-full bg-[#1D1D1F] text-white text-sm font-medium">iPhone</span>
            </div>
            <div class="flex flex-col sm:flex-row gap-3 justify-center lg:justify-start">
              <a href="#trailer" class="px-6 py-3 rounded-full bg-accent text-white font-semibold text-center hover:opacity-90 transition-opacity" data-i18n="hero.cta.trailer">Watch Trailer</a>
              <a href="#game" class="px-6 py-3 rounded-full bg-white text-[#1D1D1F] font-semibold text-center border border-gray-200 hover:bg-gray-50 transition-colors" data-i18n="hero.cta.learn">Learn More</a>
            </div>
          </div>
          <div class="flex-1 w-full max-w-xl">
            <img src="SaboTrackHD.png" alt="SaboTrack gameplay" class="w-full rounded-3xl shadow-2xl">
          </div>
        </div>
      </div>
    </section>

  </main>
```

- [ ] **Step 2: Verify in browser**

Expected:
- Gray (`#F5F5F7`) full-height section
- "SaboTrack" in Audiowide, very large; tagline below in gray
- "Apple TV" and "iPhone" as dark pill badges
- Orange "Watch Trailer" button + white-bordered "Learn More" button
- `SaboTrackHD.png` on the right at desktop width (≥1024px), below text on mobile
- IT toggle: tagline becomes "Il tracciato cambia. Il caos è tuo." and button labels update

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: hero section"
```

---

### Task 3: Trailer section

**Files:**
- Modify: `index.html` — add section inside `<main>`, after hero `</section>` and before `</main>`

**Interfaces:**
- Consumes: `translations` key `trailer.title`
- Produces: `id="trailer"` anchor target (hero CTA scrolls here)

- [ ] **Step 1: Add trailer section — insert after hero `</section>`, before `</main>`**

```html
    <!-- ── TRAILER ───────────────────────────────────────────────────────── -->
    <section id="trailer" class="bg-[#1D1D1F] py-24">
      <div class="max-w-6xl mx-auto px-6">
        <h2 class="font-display text-3xl md:text-4xl text-white text-center mb-12" data-i18n="trailer.title">Watch the Trailer</h2>
        <div class="max-w-4xl mx-auto">
          <div class="aspect-video rounded-2xl overflow-hidden shadow-2xl">
            <iframe
              src="https://www.youtube.com/embed/dQw4w9WgXcQ"
              title="SaboTrack Trailer"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
              class="w-full h-full">
            </iframe>
          </div>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Verify in browser**

Expected:
- Dark section with white "Watch the Trailer" heading
- YouTube iframe fills container at 16:9 ratio with `rounded-2xl` corners (the `overflow-hidden` on the wrapper clips the iframe corners)
- Heading switches to "Guarda il Trailer" on IT toggle
- Clicking "Watch Trailer" in the hero scrolls smoothly to this section
- **When replacing the placeholder URL:** swap `dQw4w9WgXcQ` with the real YouTube video ID

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: trailer section"
```

---

### Task 4: Design Pillars section

**Files:**
- Modify: `index.html` — add section after trailer `</section>`, before `</main>`

**Interfaces:**
- Consumes: `translations` keys `game.title`, `game.intro`, `game.pillar1.title`, `game.pillar1.body`, `game.pillar2.title`, `game.pillar2.body`, `game.pillar3.title`, `game.pillar3.body`
- Produces: `id="game"` anchor target (hero "Learn More" CTA scrolls here)
- Note: filenames with spaces are URL-encoded — `Controller%20Chaos.png`, `Comeback%20Loop.png`, `Splite%20Experience.png`

- [ ] **Step 1: Add pillars section — insert after trailer `</section>`, before `</main>`**

```html
    <!-- ── DESIGN PILLARS ────────────────────────────────────────────────── -->
    <section id="game" class="bg-white py-24">
      <div class="max-w-6xl mx-auto px-6">
        <h2 class="font-display text-3xl md:text-4xl text-[#1D1D1F] text-center mb-4" data-i18n="game.title">How It Works</h2>
        <p class="text-[#6E6E73] text-center max-w-2xl mx-auto mb-16 text-lg leading-relaxed" data-i18n="game.intro">Each race, players place objects on the track — ramps, oil slicks, spinning hammers — shaping the chaos for everyone. Score points by finishing races, triggering your own objects, and outsmarting your rivals.</p>
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">

          <div class="bg-[#F5F5F7] rounded-2xl overflow-hidden shadow-sm">
            <img src="Controller%20Chaos.png" alt="Controlled Chaos" class="w-full object-cover">
            <div class="p-6">
              <h3 class="font-display text-xl text-[#1D1D1F] mb-3" data-i18n="game.pillar1.title">Controlled Chaos</h3>
              <p class="text-[#6E6E73] text-sm leading-relaxed" data-i18n="game.pillar1.body">Every race is unpredictable, but the unpredictability always comes from deliberate player choices — not pure randomness. The track transforms lap by lap by collective will.</p>
            </div>
          </div>

          <div class="bg-[#F5F5F7] rounded-2xl overflow-hidden shadow-sm">
            <img src="Comeback%20Loop.png" alt="Comeback Loop" class="w-full object-cover">
            <div class="p-6">
              <h3 class="font-display text-xl text-[#1D1D1F] mb-3" data-i18n="game.pillar2.title">Comeback Loop</h3>
              <p class="text-[#6E6E73] text-sm leading-relaxed" data-i18n="game.pillar2.body">Players behind in the standings always have concrete tools to catch up. No one is out of the game before the last race.</p>
            </div>
          </div>

          <div class="bg-[#F5F5F7] rounded-2xl overflow-hidden shadow-sm">
            <img src="Splite%20Experience.png" alt="Split Experience" class="w-full object-cover">
            <div class="p-6">
              <h3 class="font-display text-xl text-[#1D1D1F] mb-3" data-i18n="game.pillar3.title">Split Experience</h3>
              <p class="text-[#6E6E73] text-sm leading-relaxed" data-i18n="game.pillar3.body">TV and phone are two complementary roles in the same experience. The shared screen creates social moments; the phone creates intimacy and control.</p>
            </div>
          </div>

        </div>
      </div>
    </section>
```

- [ ] **Step 2: Verify in browser**

Expected:
- White section, centered heading + intro paragraph
- Three cards in a row on desktop (≥1024px), stacked on mobile
- Each card: pillar image fills the top, Audiowide title, body text below on gray background
- Open DevTools → Console: no 404 errors for the three images
- Language toggle swaps all six text elements (3 titles + 3 bodies)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: design pillars section"
```

---

### Task 5: Screenshots section + lightbox wiring

**Files:**
- Modify: `index.html` — add section after pillars `</section>`, before `</main>`

**Interfaces:**
- Consumes: `translations` key `screenshots.title`
- Consumes: `openLightbox(src, alt)` from Task 1
- Produces: `id="screenshots"` section

- [ ] **Step 1: Add screenshots section — insert after pillars `</section>`, before `</main>`**

```html
    <!-- ── SCREENSHOTS ───────────────────────────────────────────────────── -->
    <section id="screenshots" class="bg-[#F5F5F7] py-24">
      <div class="max-w-6xl mx-auto px-6">
        <h2 class="font-display text-3xl md:text-4xl text-[#1D1D1F] text-center mb-12" data-i18n="screenshots.title">Screenshots</h2>
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
          <img src="ScreenshotPista.png"   alt="Track view"        class="w-full rounded-2xl object-cover cursor-pointer hover:opacity-90 transition-opacity aspect-video" onclick="openLightbox(this.src, this.alt)">
          <img src="ScreenshotTV.png"      alt="TV screen"         class="w-full rounded-2xl object-cover cursor-pointer hover:opacity-90 transition-opacity aspect-video" onclick="openLightbox(this.src, this.alt)">
          <img src="ScreenshotMobile1.png" alt="Mobile controller" class="w-full rounded-2xl object-cover cursor-pointer hover:opacity-90 transition-opacity aspect-video" onclick="openLightbox(this.src, this.alt)">
          <img src="ScreenshotMobile2.png" alt="Mobile strategy"   class="w-full rounded-2xl object-cover cursor-pointer hover:opacity-90 transition-opacity aspect-video" onclick="openLightbox(this.src, this.alt)">
          <img src="racetime.png"          alt="Race time"         class="w-full rounded-2xl object-cover cursor-pointer hover:opacity-90 transition-opacity aspect-video" onclick="openLightbox(this.src, this.alt)">
          <img src="sabotime.png"          alt="Sabotage phase"    class="w-full rounded-2xl object-cover cursor-pointer hover:opacity-90 transition-opacity aspect-video" onclick="openLightbox(this.src, this.alt)">
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Verify in browser**

Expected:
- Gray section with "Screenshots" heading
- 2-column grid on mobile, 4-column on desktop (6 images: first row fills 4, second row has 2 left-aligned)
- All 6 images load — check DevTools Console for 404 errors
- Clicking any image: dark overlay appears with the full image centered
- Clicking × button: overlay closes
- Clicking outside the image (on the dark background): overlay closes
- Pressing Escape: overlay closes
- While lightbox is open: body does not scroll (overflow hidden)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: screenshots section + lightbox"
```

---

### Task 6: Team section

**Files:**
- Modify: `index.html` — add section after screenshots `</section>`, before `</main>`

**Interfaces:**
- Consumes: `translations` key `team.title`
- Consumes: `.chip` CSS class (defined in Task 1 `<style>` block)
- Produces: `id="team"` section

- [ ] **Step 1: Add team section — insert after screenshots `</section>`, before `</main>`**

```html
    <!-- ── TEAM ──────────────────────────────────────────────────────────── -->
    <section id="team" class="bg-white py-24">
      <div class="max-w-6xl mx-auto px-6">
        <h2 class="font-display text-3xl md:text-4xl text-[#1D1D1F] text-center mb-16" data-i18n="team.title">Meet the Team</h2>
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-6">

          <div class="bg-[#F5F5F7] rounded-2xl p-6 flex flex-col gap-3">
            <h3 class="font-display text-base text-[#1D1D1F] leading-tight">Federico Agnello</h3>
            <p class="text-xs text-[#6E6E73]">BSc Computer Science</p>
            <div class="flex flex-wrap gap-1.5">
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Game Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Developer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Video Editor</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">3D Modeller</span>
            </div>
          </div>

          <div class="bg-[#F5F5F7] rounded-2xl p-6 flex flex-col gap-3">
            <h3 class="font-display text-base text-[#1D1D1F] leading-tight">Marco Bellante</h3>
            <p class="text-xs text-[#6E6E73]">BSc Computer Science</p>
            <div class="flex flex-wrap gap-1.5">
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">UI/UX Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Graphic Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Logo Designer</span>
            </div>
          </div>

          <div class="bg-[#F5F5F7] rounded-2xl p-6 flex flex-col gap-3">
            <h3 class="font-display text-base text-[#1D1D1F] leading-tight">Dario Giuseppe Maria Rizzo</h3>
            <p class="text-xs text-[#6E6E73]">MSc Computer Science</p>
            <div class="flex flex-wrap gap-1.5">
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Game Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Developer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">UI/UX Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">3D Environment</span>
            </div>
          </div>

          <div class="bg-[#F5F5F7] rounded-2xl p-6 flex flex-col gap-3">
            <h3 class="font-display text-base text-[#1D1D1F] leading-tight">Paolosalvatore Piazza</h3>
            <p class="text-xs text-[#6E6E73]">BSc Computer Engineering</p>
            <div class="flex flex-wrap gap-1.5">
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">UI/UX Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Developer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Logo Designer</span>
              <span class="chip px-2 py-0.5 rounded-full text-xs font-medium">Graphic Designer</span>
            </div>
          </div>

        </div>
      </div>
    </section>
```

- [ ] **Step 2: Verify in browser**

Expected:
- White section with "Meet the Team" heading
- 2×2 grid on mobile, 4-column row on desktop
- Each card: name in Audiowide, degree in small gray text, role chips in light orange with orange text
- "Meet the Team" → "Il Team" on IT toggle

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: team section"
```

---

### Task 7: Our Story / AFP section

**Files:**
- Modify: `index.html` — add section after team `</section>`, before `</main>`

**Interfaces:**
- Consumes: `translations` keys `story.title`, `story.why.heading`, `story.why.body`, `story.afp.heading`, `story.afp.body`
- Produces: `id="story"` section

- [ ] **Step 1: Add story section — insert after team `</section>`, before `</main>`**

```html
    <!-- ── OUR STORY / AFP ───────────────────────────────────────────────── -->
    <section id="story" class="bg-[#F5F5F7] py-24">
      <div class="max-w-6xl mx-auto px-6">
        <h2 class="font-display text-3xl md:text-4xl text-[#1D1D1F] text-center mb-16" data-i18n="story.title">Our Story</h2>
        <div class="flex flex-col lg:flex-row gap-12 items-start">

          <div class="flex-1 space-y-8">
            <div>
              <h3 class="font-display text-xl text-[#1D1D1F] mb-3" data-i18n="story.why.heading">Why We Built SaboTrack</h3>
              <p class="text-[#6E6E73] leading-relaxed" data-i18n="story.why.body">We wanted to create a game that feels like a party from the moment you turn it on — chaotic, social, and always fair. SaboTrack was born from our love of party games and our frustration with games that leave players feeling powerless.</p>
            </div>
            <div>
              <h3 class="font-display text-xl text-[#1D1D1F] mb-3" data-i18n="story.afp.heading">Apple Foundation Program</h3>
              <p class="text-[#6E6E73] leading-relaxed" data-i18n="story.afp.body">SaboTrack was developed as part of the Apple Foundation Program Avanzato 2026, an intensive program where selected developers build real products using Apple technologies. The experience shaped every design decision — from the asymmetric Apple TV + iPhone setup to the clean, minimal aesthetic.</p>
            </div>
          </div>

          <div class="w-full lg:w-[45%] flex-shrink-0">
            <img src="afp_avanzato_2026.JPG" alt="Apple Foundation Program Avanzato 2026 team photo" class="w-full rounded-2xl shadow-lg object-cover">
          </div>

        </div>
      </div>
    </section>
```

- [ ] **Step 2: Verify in browser**

Expected:
- Gray section with "Our Story" heading
- Two columns on desktop: text (two subsections) on left, photo on right
- Stacked on mobile: text above photo
- `afp_avanzato_2026.JPG` loads (note: uppercase `.JPG` extension — verify no 404 in DevTools Console)
- All 4 text elements (2 headings + 2 paragraphs) swap on IT toggle

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: our story / AFP section"
```

---

### Task 8: Footer + meta tags + GitHub Pages deploy

**Files:**
- Modify: `index.html` — add `<footer>` after `</main>` and meta tags inside `<head>`

**Interfaces:**
- Consumes: `translations` key `footer.rights`

- [ ] **Step 1: Add footer — insert after `</main>`, before the lightbox `<div>`**

```html
  <!-- ── FOOTER ────────────────────────────────────────────────────────── -->
  <footer class="bg-[#1D1D1F] py-10">
    <div class="max-w-6xl mx-auto px-6 flex flex-col sm:flex-row items-center justify-between gap-4">
      <div class="flex items-center gap-2">
        <img src="icon.png" alt="SaboTrack icon" class="h-6 w-6 rounded">
        <span class="font-display text-white text-sm">SaboTrack</span>
      </div>
      <p class="text-[#6E6E73] text-sm text-center" data-i18n="footer.rights">© 2026 SaboTrack. All rights reserved.</p>
    </div>
  </footer>
```

- [ ] **Step 2: Add meta tags — insert inside `<head>`, after `<title>SaboTrack</title>`**

```html
  <meta name="description" content="SaboTrack — a multiplayer party racing game for Apple TV and iPhone. Place objects, shape the chaos, and race to the top.">
  <meta property="og:title" content="SaboTrack">
  <meta property="og:description" content="A multiplayer party racing game for Apple TV and iPhone.">
  <meta property="og:image" content="SaboTrackHD.png">
  <meta property="og:type" content="website">
  <meta name="twitter:card" content="summary_large_image">
  <link rel="icon" type="image/png" href="icon.png">
  <link rel="apple-touch-icon" href="icon.png">
```

- [ ] **Step 3: Verify footer in browser**

Expected:
- Dark footer at page bottom: icon + "SaboTrack" on left, copyright on right (desktop); stacked on mobile
- Copyright text changes on IT toggle: "© 2026 SaboTrack. Tutti i diritti riservati."
- Browser tab shows `icon.png` as favicon

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: footer + meta tags + favicon"
```

- [ ] **Step 5: Push to GitHub and enable Pages**

```bash
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

Then in GitHub: **Settings → Pages → Source: Deploy from branch → Branch: `main` → Folder: `/ (root)` → Save**.

- [ ] **Step 6: Verify live site**

Open `https://YOUR_USERNAME.github.io/YOUR_REPO/` (allow ~60 seconds for first deploy). Verify:
- All images load (no broken image icons)
- Language toggle works and persists on reload
- Trailer iframe loads
- Lightbox opens and closes on screenshots
- Smooth scroll from hero CTAs to their target sections
- Site is readable on mobile (test with DevTools device emulation at 390px width)
