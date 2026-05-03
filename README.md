# Patel's Fitness Club - Website

Premium single-page gym website for **Patel's Fitness Club**, Rajkot.

## Live Preview

Open `index.html` directly in a browser - no build step or server required.

## Tech Stack

- **Pure HTML/CSS/JS** - single `index.html` file, zero dependencies
- **Fonts**: Bebas Neue, Inter, JetBrains Mono (loaded from Google Fonts)
- **No frameworks, no build tools, no npm** - just open and deploy

## Design

- Dark brutalist/editorial aesthetic (sharp edges, geometric patterns)
- Fully responsive (desktop, tablet, mobile)
- Sections: Hero, About, Programs, Trainers, Pricing, Results, Gallery, Contact (Google Maps), Footer
- Auto-swapping image slideshows for gym interior and program cards
- Trainer card flip animation on hover
- IntersectionObserver scroll reveals, count-up animations, floating particles

## Pricing Plans

| Plan | Price | Duration |
|------|-------|----------|
| Basic | Rs 3,000 | 1 Month |
| Pro | Rs 6,000 | 3 Months |
| Elite | Rs 8,000 | 6 Months |
| Annual | Rs 15,000 | 1 Year |

## Images

- `hero-bg.png` - Hero section background
- `gym-1.jpeg` to `gym-7.jpeg` - Gym interior photos (about section slideshow)
- `st-1.jpeg` to `st-4.jpeg` - Strength training photos (programs section)
- `cardio-1.jpeg` to `cardio-3.jpeg` - Cardio section photos (programs section)

## Hosting on Google (Firebase Hosting)

### Prerequisites
1. Install Node.js (https://nodejs.org)
2. Install Firebase CLI: `npm install -g firebase-tools`
3. Log in: `firebase login`

### Setup
```bash
firebase init hosting
```
When prompted:
- Project directory: `.` (current directory)
- Public directory: `.` (since index.html is at root)
- Single-page app: **Yes**
- Overwrite index.html: **No**

### Deploy
```bash
firebase deploy --only hosting
```

Your site will be live at: `https://your-project-id.web.app`

### Custom Domain
1. Go to Firebase Console > Hosting > Add custom domain
2. Add your domain (e.g., `patelsfitness.com`)
3. Update DNS records as instructed by Firebase

## Alternative: Google Cloud Storage Static Hosting

```bash
# Create a bucket (use your domain name as bucket name for custom domain)
gsutil mb gs://www.patelsfitness.com

# Set bucket as public website
gsutil web set -m index.html gs://www.patelsfitness.com

# Upload all files
gsutil -m cp -r . gs://www.patelsfitness.com

# Make files public
gsutil iam ch allUsers:objectViewer gs://www.patelsfitness.com
```

## File Structure

```
/
├── index.html          # Complete website (HTML + CSS + JS)
├── hero-bg.png         # Hero background image
├── gym-*.jpeg          # Gym interior photos (7 images)
├── st-*.jpeg           # Strength training photos (4 images)
├── cardio-*.jpeg       # Cardio photos (3 images)
├── firebase.json       # Firebase hosting config
├── .firebaserc         # Firebase project config
├── README.md           # This file
└── .gitignore          # Git ignore rules
```

## Google Maps

The contact section embeds a live Google Maps iframe pointing to the real location of Patel's Fitness Club in Rajkot. No API key required (uses embed mode).
