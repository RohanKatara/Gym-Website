# Patel's Fitness Club - Website Project

## Project Overview
Single-page premium gym website for "Patel's Fitness Club" — a dark, brutalist/editorial aesthetic fitness site with sharp edges, geometric patterns, and zero external dependencies.

## Tech Stack
- **Single file**: `index.html` (self-contained HTML + CSS + JS)
- **Animation**: Vanilla CSS keyframes + IntersectionObserver (no libraries)
- **Fonts**: Bebas Neue (display) + Inter (body) + JetBrains Mono (labels/mono) from Google Fonts
- **No build step** — pure vanilla HTML/CSS/JS, zero CDN dependencies

## Design System
- **Theme**: Dark, brutalist/editorial (sharp edges, no border-radius)
- **Background**: #0A0A0A (--bg), #141420 (--card), #1A1A2E (--card-2)
- **Accent**: #E63946 (--red) primary, #FFD700 (--gold) sparingly
- **Text**: #FFFFFF (--text), #9A9AA8 (--muted), #6A6A78 (--dim)
- **Typography**: Bebas Neue for headings, Inter for body, JetBrains Mono for labels/kickers
- **Cards**: Sharp-edged with 1px borders (no glassmorphism/blur on cards)
- **Section labels**: JetBrains Mono with diamond (◆) prefix, red color, 0.4em letter-spacing

## Key Decisions Made

### User Preferences
- **No custom cursor** — uses subtle radial gradient glow that follows mouse (not a cursor replacement)
- **Bento grid gallery** — replaced horizontal pinned scroll with a 4-column bento grid layout
- **Hero headline**: "PATEL'S FITNESS CLUB" with "FITNESS" as text-stroke outline and "CLUB" in red

### Pricing (confirmed by user)
- Basic: Rs 3,000/month
- Pro: Rs 6,000/quarter (featured/highlighted with scale)
- Elite: Rs 8,000/6 months
- Annual: Rs 15,000/year

### Architecture Decision
- Single HTML file for simplicity and portability
- Zero external libraries (no GSAP, no Lenis, no CDN dependencies)
- All animations done with CSS @keyframes and vanilla JS IntersectionObserver

## Sections (in order)
1. Preloader (rotating diamond "P" mark + animated loading bar)
2. Navigation (transparent -> solid on scroll, mobile hamburger menu overlay)
3. Hero (full viewport, floating particles, stats bar at bottom, parallax BG, hero-bg.png)
4. About (split layout with auto-swapping gym interior slideshow, 4 feature cards in grid)
5. Programs (4 cards: Strength Training, Cardio & HIIT with image slideshows; Personal Training, Weight Loss with placeholders)
6. Trainers (4 flip-cards with 3D Y-axis rotation on hover, silhouette placeholders)
7. Pricing (4 tiers, Pro highlighted with scale(1.04) + red border)
8. Results (before/after split cards, 3 counters, single-direction marquee testimonials)
9. Gallery (bento grid with eye icon hover reveal)
10. Contact (Google Maps embed with dark filter + 4 info blocks, no form)
11. Footer (house mantra quote section, 4-column link grid, socials)
12. Mobile sticky CTA (fixed bottom bar on small screens)

## Animation Features
- CSS fadeUp keyframes on hero elements (staggered delays)
- IntersectionObserver-based scroll reveals (.reveal class)
- Floating particles (dots, squares, lines) with CSS animation
- Cursor glow (radial gradient following mouse, desktop only)
- 3D perspective hover on program cards (CSS transition)
- Flip-card trainers (180° Y-axis rotation on hover via perspective + preserve-3d + backface-visibility)
- Auto-swapping image slideshows (about section gym photos, program card images) via setInterval + opacity
- Infinite marquee testimonials (single direction, duplicated for seamless loop)
- Count-up number animations (vanilla JS + IntersectionObserver)
- Pulsing map pin + loader mark animations
- Hero parallax (translateY + subtle scale on scroll)
- prefers-reduced-motion: disables all animations EXCEPT trainer flip (overridden)

## Known Gotcha: 3D Flip Animation + Reduced Motion

**Problem:** CSS 3D card flip animations (rotateY, preserve-3d, backface-visibility) would not work on this site due to two compounding issues:

1. **`body { overflow-x: hidden }` flattens 3D context** — Any ancestor with `overflow: hidden` breaks `transform-style: preserve-3d` for its descendants. This means you cannot rely on a global preserve-3d chain.

2. **User's Windows has `prefers-reduced-motion` enabled** — The site's `@media (prefers-reduced-motion: reduce)` rule sets `transition-duration: 0.01ms !important` on ALL elements (`*, *::before, *::after`), killing any transition/animation.

**Solution:**
- Apply `perspective: 1000px` directly on the card's immediate parent (`.trainer`) — this creates a **local** 3D rendering context that isn't affected by body's overflow.
- Apply `transform-style: preserve-3d` only on `.trainer-inner` (one level below the perspective container).
- Use `backface-visibility: hidden` on both faces.
- In the `prefers-reduced-motion` media query, add a specific override: `.trainer-inner { transition-duration: 0.8s !important; }` to exempt the flip from the blanket duration kill.

**Rule:** When adding any new transition/animation that must always be visible, add its element to the reduced-motion override list.

## Images
- `hero-bg.png` — Hero section background
- `gym-1.jpeg` to `gym-7.jpeg` — Gym interior photos (about section auto-swapping slideshow)
- `st-1.jpeg` to `st-4.jpeg` — Strength training photos (programs section slideshow)
- `cardio-1.jpeg` to `cardio-3.jpeg` — Cardio section photos (programs section slideshow)
- Trainer cards and remaining sections still use dark gradient/pattern placeholders with silhouettes

## Hosting
- **GitHub repo**: https://github.com/RohanKatara/Gym-Website
- **Firebase Hosting** config included (`firebase.json`, `.firebaserc`) — ready to deploy
- No build step needed; deploy the root directory as-is

## Content Source
- Original design exported from Claude AI Design (claude.ai/design) as handoff bundle
- Mobile hamburger menu added during implementation (not in original design export)
