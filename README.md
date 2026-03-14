# 🏮 The Last Lighthouse Keeper

> *"Write a story that breathes. Where every word moves. Where pages turn with grace. Where code becomes narrative."*

An interactive cinematic storytelling experience built for the **Interactive Storytelling Platform with GSAP Animation** hackathon. A short story about two best friends, a lighthouse, and the light that never goes out — told through scroll-driven animation, kinetic typography, and a live canvas ocean.

---

## 🌊 Live Demo

👉 https://the-last-lighthouse-keeper.vercel.app/

---

## 📖 The Story

**The Last Lighthouse Keeper** follows Marcus and Eli, childhood friends on the fictional Heron's Bluff island. The story spans four decades:

| Chapter | Title | Summary |
|---|---|---|
| One | Two Boys, One Island | Two best friends grow up sneaking into an automated lighthouse, naming ships on the horizon |
| Two | The Night Everything Changed | A storm named Agnes knocks out the lighthouse beam — the boys run into the dark to bring it back |
| Three | The Slow Distance | Life pulls them apart across decades of silence, parallel lives, and drifting letters |
| Epilogue | The Light That Stays | Marcus retires and returns. Eli was already at the dock. |

**Themes:** friendship across time, the cost of leaving, the quiet heroism of staying, and the idea that some lights can't be automated.

**Read time:** ~3–4 minutes

---

## ✨ Features

### Animations & Interactions
- **GSAP Master Timeline** — all hero entrance animations sequenced via a single coordinated timeline
- **ScrollTrigger** — every chapter reveal, heading, pull quote, and illustration fires on scroll
- **Lightning Strike System** — screen flash + full-body page shake (`gsap.timeline` rapid X-axis sequence) triggered when the storm section enters view
- **Sea Sway** — continuous `requestAnimationFrame` ocean motion applied to all chapter content blocks, each with unique phase offset
- **Kinetic Typography** — per-character `splitHeadingChars()` animation with random rotation, scale, and stagger on every chapter heading
- **Word-by-word text reveal** — story paragraphs animate word by word with 3D `rotationX` on scroll enter
- **Pull quote border draw** — animated left border grows top-to-bottom on scroll, words stagger in independently
- **Typewriter chapter labels** — letter-spacing expansion reveal on scroll
- **Glitch text effect** — CSS `::before`/`::after` clip-path RGB split on the storm word "AGNES"

### Canvas & Visual
- **Live ocean canvas background** — `requestAnimationFrame` loop with 180 twinkling star particles, 60 bioluminescent plankton, 5-layer multi-frequency wave system, and 4 drifting fog blobs
- **Storm canvas** — dedicated canvas inside the storm section with procedural cloud blobs, rain streaks, and branching lightning bolts drawn frame-by-frame
- **Lighthouse SVG** — fully hand-coded, animated lighthouse with layered glow halo, rotating beam, pulsing light source, keeper's cottage with glowing windows, gallery railing, and 3 animated wave paths

### UX & Accessibility
- Custom gold cursor with lagging ring follower (`mix-blend-mode: screen`)
- Scroll progress bar (teal → gold gradient)
- Chapter navigation dots (scroll-aware, keyboard navigable, ARIA labelled)
- Semantic HTML with ARIA labels throughout
- Keyboard navigation support on all interactive elements
- `drop-cap` on chapter openers
- Mobile responsive — nav dots hidden on small screens, fluid type scaling via `clamp()`

### Audio
- Web Audio API ambient ocean synth — 6 layered oscillators (sine + triangle) with LFO frequency modulation, togglable with fade in/out

---

## 🛠️ Tech Stack

| Technology | Usage |
|---|---|
| **HTML5** | Semantic structure, SVG illustrations, ARIA accessibility |
| **CSS3** | Custom properties, clip-path glitch effect, drop-cap, animations |
| **JavaScript (ES6+)** | Canvas rendering, Web Audio API, RAF loops, DOM manipulation |
| **GSAP 3.12.5** | All scroll and entrance animations, timelines, stagger |
| **GSAP ScrollTrigger** | Scroll-based chapter reveals, progress bar, section detection |
| **GSAP TextPlugin** | Text animations |
| **Google Fonts** | Playfair Display · Crimson Pro · Space Mono |

**Zero build tools. Zero frameworks. Zero dependencies beyond GSAP.**

---

## 📁 File Structure

```
/
├── index.html        # Everything — HTML, CSS, JS in one file
└── README.md         # This file
```

Single-file architecture — intentional. The entire experience lives in one `index.html`. Clean, portable, instantly deployable.

---

## 🚀 Deployment

### GitHub Pages (recommended)

1. Fork or clone this repo
2. Go to **Settings → Pages**
3. Set source to `main` branch, `/ (root)`
4. Save — your site will be live at `https://yourusername.github.io/repo-name`

### Local

```bash
# Clone
git clone https://github.com/yourusername/last-lighthouse-keeper.git
cd last-lighthouse-keeper

# Open directly — no build step needed
open index.html

# Or serve locally to avoid any CORS issues
npx serve .
# or
python3 -m http.server 8000
```

---

## 🏗️ Architecture Notes

### Why single file?
Hackathon constraint: HTML, CSS, JavaScript only. Single file keeps deployment dead simple and eliminates any asset loading latency.

### GSAP animation strategy
All animations follow a strict pattern:
1. `gsap.set()` establishes initial state (avoids CSS/GSAP opacity conflict)
2. `gsap.to()` or `gsap.timeline()` drives the animation forward
3. `ScrollTrigger` fires each sequence at the right scroll position

Never use `.from()` on elements that have `opacity: 0` in CSS — GSAP reads the computed style as the target, not the destination, and the element stays invisible.

### Canvas rendering
Two separate canvases run simultaneously:
- `#ocean-canvas` — fixed background, always running, reacts to scroll position to shift color temperature
- `#storm-canvas` — inside the storm section, only active when section is in viewport (performance gate via ScrollTrigger)

### Sea sway
Each `.sea-sway` chapter block gets its own `requestAnimationFrame` loop with a unique `phase` offset derived from its index. Uses `Math.sin()` for Y translation and a secondary slower sin for `rotationZ`. Amplitude is 2–3px — subtle enough to feel like reading on water, not like a carnival ride.

### Lightning system
`window.triggerLightning()` is a globally exposed function called by:
- The storm canvas when it draws a bolt (randomized timer ~60–150 frames)
- ScrollTrigger `onEnter` for the storm section (3 pre-timed strikes: 400ms, 1100ms, 2500ms)

Each call triggers a `gsap.timeline()` rapid X-axis shake on `body` + a CSS overlay flash via `rgba` background animation.

### Text splitting
`splitHeadingChars(el)` safely walks the DOM tree character by character, wrapping each in a `<span class="char">`, while preserving `<em>`, `<strong>`, and `<br>` element structure. This avoids the common mistake of `el.innerHTML = ''` which destroys nested tags.

---

## ⚡ Performance

| Metric | Target | Approach |
|---|---|---|
| Page load | < 3s | Single file, GSAP via CDN, no images |
| Animation | 60fps | `will-change` on animated elements, canvas RAF |
| Lighthouse score | 90+ | Semantic HTML, ARIA, no render-blocking resources |
| Canvas perf | Gated | Storm canvas only runs when section is in viewport |

---

## 🎨 Design System

```css
--deep-navy:   #050d1a   /* Page background */
--ocean-dark:  #0a1f35   /* Deep sections */
--teal:        #1a7a8a   /* Structural accents */
--teal-light:  #2eb8cc   /* Labels, UI chrome */
--gold:        #c9a227   /* Story emphasis */
--gold-light:  #f0c84b   /* Light source, glow */
--cream:       #f5ead8   /* Primary text */
--fog:         #a8c5d0   /* Secondary text */

/* Fonts */
--font-display: 'Playfair Display'   /* Headings, pull quotes */
--font-body:    'Crimson Pro'        /* Story text */
--font-mono:    'Space Mono'         /* Labels, UI elements */
```

---

## 📝 Story Credits

**Story:** Original — written for this project
**Illustrations:** Hand-coded SVG — no external images
**Audio:** Procedurally generated — Web Audio API synthesizer

---

## 📄 License

MIT — use it, fork it, build on it.

---

*Built with GSAP · HTML · CSS · JavaScript*
*Interactive Storytelling Platform Hackathon*
