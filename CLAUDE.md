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

### Pricing (updated by user)
- Monthly: Rs 3,000/month
- Quarterly: Rs 8,000/3 months
- Half Yearly: Rs 12,000/6 months (featured/highlighted with scale + "Best Value" badge)
- Annual: Rs 18,000/year

### Architecture Decision
- Single HTML file for simplicity and portability
- Zero external libraries (no GSAP, no Lenis, no CDN dependencies)
- All animations done with CSS @keyframes and vanilla JS IntersectionObserver

## Sections (in order)
1. Preloader (rotating diamond "P" mark + animated loading bar)
2. Navigation (transparent -> solid on scroll, mobile hamburger menu overlay)
3. Hero (full viewport, floating particles, stats bar at bottom, parallax BG, hero-bg.png)
4. About (split layout with auto-swapping gym interior slideshow, 4 feature cards in grid)
5. Programs (4 cards: Strength Training, Cardio & HIIT with real photo slideshows; Personal Training + Weight Loss with HD stock images from Pexels)
6. Trainers (5 flip-cards with real photos + 3D Y-axis rotation on hover)
7. Pricing (4 tiers, Half Yearly highlighted with scale(1.04) + red border + "Best Value" badge)
8. Results (4 real before/after transformation cards with actual member photos, 3 counters, auto-scrolling marquee with 10 real Google reviews)
9. Gallery "Our Facility" (bento grid with eye icon hover reveal, includes steam room + locker room photos)
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
- prefers-reduced-motion: disables decorative animations but exempts interactive feedback (0.25s), scroll reveals (0.5s fade), trainer flip, mobile menu, and testimonials marquee

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

## Known Gotcha: Testimonials Marquee + Reduced Motion

**Problem:** The testimonials auto-scrolling marquee was disabled by `prefers-reduced-motion` because the blanket `animation-duration: 0.01ms !important` rule killed it, and there was also an explicit `.testimonials-track { animation: none; }` in the reduced-motion block.

**Solution:** Override the reduced-motion rule with `.testimonials-track { animation: marquee 120s linear infinite !important; animation-iteration-count: infinite !important; }` to keep the marquee running. Also added `.testimonials:hover .testimonials-track { animation-play-state: paused; }` for usability.

## Images
- `hero-bg.png` — Hero section background
- `gym-1.jpeg` to `gym-7.jpeg` — Gym interior photos (about section auto-swapping slideshow)
- `st-1.jpeg` to `st-4.jpeg` — Strength training photos (programs section slideshow)
- `cardio-1.jpeg` to `cardio-3.jpeg` — Cardio section photos (programs section slideshow)
- `personal-training.jpeg` — HD stock photo from Pexels (trainer assisting with dumbbell curls)
- `weight-loss.jpeg` — HD stock photo from Pexels (woman on treadmill)
- `steam-room.png` — Steam room photo (Gemini watermark removed via OpenCV inpainting)
- `locker-room-1.jpeg`, `locker-room-2.jpeg` — Locker room photos
- `t1-before.png`, `t1-after.png` — Transformation 1 (split from composite, text cropped out)
- `t2-before.png`, `t2-after.png` — Transformation 2 (split from composite)
- `t3-before.png`, `t3-after.png` — Transformation 3 (split from composite)
- `t4-before.jpeg`, `t4-after.jpeg` — Transformation 4 (separate files, use object-fit:contain)
- `trainer-ravi-rathod.jpeg` — Trainer: Ravi Rathod (CPT Level 2 INFS, 10+ yrs, 60+ transformations)
- `trainer-seeraj.png` — Trainer: Seeraj Dalwani (MIFF Certified, 10+ yrs, 40+ transformations)
- `trainer-brijesh.png` — Trainer: Brijesh Jain (Pro Ultimate Academy, 4+ yrs)
- `trainer-smit.jpeg` — Trainer: Smit (ACE-CPT, 5+ yrs) — converted from HEIC
- `trainer-rahul.jpeg` — Trainer: Rahul (ISSA-CPT, 6+ yrs)

## Trainers (5 total, grid is 5 columns)
1. **Ravi Rathod** — CPT Level 2 (INFS), 10+ years, 60+ transformations
2. **Seeraj Dalwani** — MIFF Certified, 10+ years, 40+ transformations (object-position: center 30%)
3. **Brijesh Jain** — Pro Ultimate Academy, 4+ years (object-position: center center)
4. **Smit** — ACE-CPT, 5+ years (AI-generated bio)
5. **Rahul** — ISSA-CPT, 6+ years (AI-generated bio, object-position: center center)

## Testimonials
- 10 real 5-star Google reviews replace the original placeholder testimonials
- Reviewers: Dev Patel, Yash Thadeshwar, Ketan Patel, Danzo Kotadia, Narendra Patel, Rahul Pawar, Bhavy Madhani, Pooja Patel, Tisha Soni, Meet N Popat
- Each shows 5-star rating + "Google Review" label
- Auto-scrolling marquee (120s duration), pauses on hover
- Duplicated for seamless infinite loop

## Transformations (4 real before/after cards)
1. 3 Months, 99.8kg to 80.7kg (composite split, text cropped)
2. 1 Year, 31kg loss (composite split)
3. Weight loss transformation (composite split)
4. Fitness transformation (separate before/after images, uses object-fit:contain)

## About Section Feature Boxes
- 4 boxes are wrapped in `<a>` tags linking to relevant sections
- Expert Trainers → #trainers
- Premium Equipment → #gallery
- Steam & Recovery → #gallery
- 3-Floor Facility → #gallery
- CSS: `.features a { display: contents; }` to preserve grid layout

## Text Content Style
- No em-dashes or en-dashes used anywhere in text content (removed per user preference)

## Hosting
- **GitHub repo**: https://github.com/RohanKatara/Gym-Website
- **Firebase Hosting** config included (`firebase.json`, `.firebaserc`) — ready to deploy
- No build step needed; deploy the root directory as-is

## Mobile-First UX Overhaul

### Responsive Breakpoints
- **1024px**: Mobile experience begins (hamburger menu, hero restructuring, 2-column grids)
- **640px**: Small phone (single-column grids, stacked CTAs, sticky CTA visible, testimonial marquee at 90s speed)

### Known Gotcha: Hero Section Mobile Layout

**Problem:** On desktop, the hero uses `height: 100vh; overflow: hidden` with the stats bar `position: absolute; bottom: 0`. On any viewport ≤1024px, the content stack (title + tag + CTAs + stats) can exceed the viewport height, and `overflow: hidden` clips it. The stats bar's `z-index: 3` also paints on top of hero-inner's `z-index: 2`, causing visual overlap.

**Solution (applied at 1024px breakpoint):**
- Hero becomes `height: auto; min-height: 100dvh; display: flex; flex-direction: column; align-items: stretch; overflow: visible`
- Hero-inner becomes `flex: 1; justify-content: flex-end` (content pushed to bottom, above stats)
- Stats bar becomes `position: relative` (flows naturally instead of absolute overlay) with `left/right/bottom: auto; width: 100%`
- Hero-bg gets explicit `width: 100%; height: 100%` to cover the hero when it grows beyond viewport

**Rule:** Hero layout changes MUST be at the 1024px breakpoint, NOT 640px. The hero overflow issue affects ALL mobile/tablet widths, not just small phones. The hamburger menu appears at 1024px, which marks the start of the mobile experience.

### Mobile Menu Animation
- Curtain reveal via `clip-path: inset(0 0 100% 0)` transitioning to `clip-path: inset(0)`
- Staggered link entrance: each `a:nth-child(n)` has incremental `transition-delay`
- Swipe-to-close gesture (horizontal swipe > 80px)
- Smooth anchor scroll: menu closes first, then `scrollIntoView` after 300ms delay
- Reduced-motion exemptions: `.mobile-menu { transition-duration: 0.3s !important; }` and `.mobile-menu a { transition-duration: 0.2s !important; }`

### Touch Interactions
- **Trainer cards**: Tap-to-flip via `.trainer.flipped` class (JS toggles on click for touch devices). Closes other open cards. Subtitle text changes "Hover" to "Tap" on touch devices.
- **Touch interaction system**: JS-based `.touched` class (event-delegated, single click listener on document) provides sustained visual feedback on tap for `.feature, .program, .tier, .before-after, .gallery-item, .info-block, .btn`. Clears on scroll (100ms debounce) or tap outside. See "Mobile Touch Interaction System" section below for full details.
- **Program cards**: Desktop 3D hover in `@media (hover: hover)`. Touch gets `.touched` state (translateY + border-color + bg) via JS + `:active` fallback.
- **Gallery eye icons**: Hidden by default at all sizes. Desktop reveals on hover. Mobile reveals via `.touched` class on tap.
- **Two-layer touch feedback**: `@media (hover: none)` block provides `.touched` (sustained, mirrors desktop hover) + `:active` (momentary press) for all interactive elements
- **Tap highlight removed**: `-webkit-tap-highlight-color: transparent` on `*` selector

### Mobile Performance Optimizations
- **Hero parallax disabled on mobile**: JS checks `window.matchMedia('(hover: hover) and (pointer: fine)')` — only runs `translateY`/`scale` transform on desktop
- **Passive scroll listener**: `{ passive: true }` on the main scroll handler
- **Fewer particles on mobile**: 8 instead of 22 (uses same `isMobile` media query check)
- **Lazy loading**: `loading="lazy"` on all images except `gym-1.jpeg` (first visible in about section)

### Mobile UX Additions
- **Back-to-top button**: Fixed position, 44px touch target, appears after 1 viewport of scroll, sits above sticky CTA at 640px (`bottom: 80px`). Reduced-motion exemption added.
- **Scroll progress bar**: 2px red bar at bottom of nav (`#scrollProgress`), width updated in the scroll handler
- **Testimonials marquee at ≤640px**: CSS marquee at 90s speed (slower than desktop 120s). JS pauses on touchstart, resumes 4s after touchend
- **Slideshow swipe**: Touch swipe (>50px) advances/reverses slides and resets auto-advance timer

### Mobile Typography & Spacing (640px)
- Preloader scaled down: 64px mark, 140px bar, 10px text
- Section padding reduced: 100px to 64px vertical
- Pricing/counter padding: 32px/24px
- Testimonials: 300px width, 24px padding, 14px quote font
- Trainer cards: `height: auto; min-height: 360px` (prevents content clipping)
- Counter numbers: `clamp(56px, 14vw, 88px)`

### Reduced-Motion Exemption Checklist
When adding new transitions/animations that must always work, add overrides inside the `prefers-reduced-motion` block:

| Element | Override | Why |
|---------|----------|-----|
| `.trainer-inner` | `transition-duration: 0.8s !important` | Flip must work |
| `.trainer-socials a` | `transition-duration: 0.2s !important` | Link hover |
| `.mobile-menu` | `transition-duration: 0.3s !important` | Navigation must open/close |
| `.mobile-menu a` | `transition-duration: 0.2s !important` | Links must appear |
| `.back-to-top` | `transition-duration: 0.2s !important` | Must fade in/out |
| `.testimonials-track` | `animation: marquee 120s linear infinite !important` | Must auto-scroll (desktop) |
| `.feature`, `.feature::after`, `.program`, `.program::before`, `.program .program-link`, `.tier`, `.before-after`, `.gallery-item`, `.gallery-item::after`, `.gallery-item .eye`, `.info-block`, `.btn`, `.sticky-cta` | `transition-duration: 0.25s !important` | Interactive feedback must work |
| `.reveal` | `transition-duration: 0.5s !important; transform: none !important` | Scroll fade-in (no motion, fade only) |

### Nav Border Transition
`nav.topnav` has `border-bottom: 1px solid transparent` in its base state so the border animates smoothly to `var(--line)` on `.scrolled` instead of snapping.

## Mobile Touch Interaction System

### How It Works
A JS-based `.touched` class system provides sustained visual feedback on tap for mobile/touch devices, replacing unreliable CSS `:hover` sticky behavior. This runs only on mobile (`isMobile` check).

- **Event delegation**: Single `click` listener on `document` using `.closest()` to match targets
- **Targets**: `.feature, .program, .tier, .before-after, .gallery-item, .info-block, .btn`
- **Exclusion**: `.trainer` cards are excluded (they have their own `.flipped` toggle system)
- **Only one `.touched` at a time**: Previous element is cleared when a new one is tapped
- **Clears on scroll**: 100ms debounced scroll listener removes `.touched` so cards don't stay highlighted while scrolling
- **Clears on tap outside**: Tapping outside any target removes the current `.touched`

### CSS Touch States (inside `@media (hover: none)`)
Two layers of feedback:
1. **`.touched` (sustained)**: Applied by JS on tap, mirrors desktop `:hover` (lift, shadow, border, color)
2. **`:active` (momentary)**: CSS-only press indication during finger contact

### Testimonial Marquee on Mobile
At 640px, testimonials use CSS marquee (`animation: marquee 90s linear infinite`) instead of the previous `animation: none` static state. JS pauses the marquee on `touchstart` and resumes 4 seconds after `touchend`.

### Gallery Tap-to-Reveal on Mobile
At 640px, gallery eye icons and overlays are hidden by default (not forced visible). They reveal via `.touched` class on tap and hide when tapping elsewhere or scrolling.

## Content Source
- Original design exported from Claude AI Design (claude.ai/design) as handoff bundle
- Mobile hamburger menu added during implementation (not in original design export)
