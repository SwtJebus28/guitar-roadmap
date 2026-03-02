# Guitar Roadmap — Project Context for Claude Agent

## What This Project Is

A Progressive Web App (PWA) called **"Guitar Roadmap — Breaking Through the Plateau"** — a 40-week structured guitar learning plan designed for a specific user profile. It's built as a single-page HTML/CSS/JS application that installs like a native app on iOS, Android, and desktop.

## The User

- Self-taught guitarist based in the Shenandoah Valley, Virginia (prime bluegrass territory)
- Plays by ear, knows many chords (open and barre), can improvise over pentatonic/blues scales
- **Gaps:** Music theory vocabulary, fretboard note memorization, major scale fluency, chord construction knowledge, bluegrass-specific techniques
- Subscribes to the **Gibson Learn & Play guitar app** — the Gibson App has complementary Guides and lessons (especially in its "Guides" section covering genres like Country, Americana, theory, scales, CAGED, etc.) but doesn't provide a structured intermediate roadmap like this project does
- Wants bite-sized, building-block lessons similar to Sonora Guitar / Banjo Ben style teaching

## Architecture

```
guitar-app/
├── index.html      ← Entire app (single-file: HTML + CSS + JS + all data)
├── manifest.json   ← PWA manifest for installability
├── sw.js           ← Service worker for offline capability
└── CONTEXT.md      ← This file
```

### Why Single-File?
The app is intentionally built as a self-contained single HTML file with embedded CSS and JS. All 40 weeks of curriculum data, resource links, and reference tables are embedded as JavaScript objects. This makes it trivially portable, easy to host anywhere (even from a local file), and simple to maintain.

### PWA Features
- **Installable**: manifest.json enables "Add to Home Screen" on iOS/Android/desktop
- **Offline-capable**: Service worker caches all assets
- **Standalone mode**: Runs without browser chrome (looks like a native app)
- **Safe area support**: Handles iPhone notch/home indicator via `env(safe-area-inset-*)`

## The 40-Week Curriculum

### Phase 1 — "The Fretboard Finally Makes Sense" (Weeks 1–6)
Fretboard memorization: natural notes → sharps/flats → octave shapes (6→4, 5→3, 4→2) → root note identification in chord shapes. Milestone: name any note on any string, find notes in 3+ positions.

### Phase 2 — "The Major Scale — Your New Best Friend" (Weeks 7–14)
W-W-H-W-W-W-H formula → G major in multiple positions → intervals (root through 7th) → scale degree numbering → major scale in G, C, D → improvising with major scale → connecting pentatonic to major (pentatonic = major minus 4th and 7th).

### Phase 3 — "How Chords Are Built" (Weeks 15–22)
Triads (R-3-5) → harmonizing major scale (I-ii-iii-IV-V-vi-vii°) → I-IV-V progressions → adding vi and ii → dominant 7th chords → CAGED system introduction → song analysis with Roman numerals.

### Phase 4 — "Bluegrass Foundations" (Weeks 23–30)
Boom-chuck rhythm → bass runs → the G run → Carter-style melody/bass → cross-picking basics → chord tone targeting for solos → bluegrass licks in G → full jam track integration.

### Phase 5 — "Deepening Your Musicianship" (Weeks 31–40)
Nashville Number System → capo transposition → relative minors → modes (Mixolydian for bluegrass) → ear training (intervals and chord quality) → double stops (3rds and 6ths) → connecting all 5 CAGED positions → full bluegrass tune performance → self-assessment.

## Curated Resources (embedded in the app)

Each phase has vetted resource links from:
- **JustinGuitar** — Free structured lessons, note trainer
- **Bryan Sutton / ArtistWorks** — 10x IBMA Guitar Player of the Year
- **Molly Tuttle** — Cross-picking lesson (Acoustic Guitar Magazine)
- **Lessons With Marcel** — Modern bluegrass YouTube
- **Country Guitar Online** — Free tab with every lesson
- **Peghead Nation** — Scott Nygaard's structured courses
- **Tunefox** — Interactive tab with speed control
- **Fretjam** — Free theory and fretboard exercises
- **Sound Guitar Lessons** — Theory-driven chord/scale series
- **Brandon Johnson Guitar** — Bluegrass runs and licks
- **Alex Graf Music** — Boom-chuck rhythm deep dives
- **Premier Guitar / Sweetwater** — Nashville Number System guides
- **FaChords** — Interactive number system tables

## Data Storage

The app uses **localStorage** for persistence (this is a standalone PWA, not an embedded artifact):
- `guitar-progress` — JSON object: `{ "1": true, "2": false, ... }` keyed by week number
- `guitar-open` — JSON object tracking which phase accordions are open/closed

## Design System

- **Typography**: Lora (serif, headings/quotes), Source Sans 3 (body), JetBrains Mono (data/labels)
- **Color palette**: Warm earthy tones — dark brown header (#2c1810), cream backgrounds (#faf8f5), saddle brown accents (#8b4513, #c4813a)
- **Phase colors**: Green → Blue → Brown → Red → Purple (one per phase)
- **Layout**: Mobile-first, max-width 680px for tablets/desktop, bottom tab navigation with 4 tabs (Plan, Resources, Keys, Tips)
- **Interaction**: Collapsible phase accordions, checkbox progress tracking, progress bar at top

## The 4 Tabs

1. **Plan** — The 40-week curriculum with collapsible phases, week cards, checkboxes, and per-phase resources
2. **Resources** — YouTube channels and websites directory, plus ear training app recommendations
3. **Keys** — Reference tables: harmonized major scale in G/C/D/A/E, capo transposition chart, major scale formula, harmonized scale pattern
4. **Tips** — Practice advice, motivational guidance, progress reset button

## Possible Future Enhancements

Ideas discussed or implied during development:
- **Gibson App integration**: Map specific Gibson App guide/lesson titles to each week (requires user to provide guide names from inside the app, since Gibson doesn't publish their catalog externally)
- **Practice timer**: Built-in 5/15/10 minute timer for the daily practice structure
- **Notes/journal**: Per-week text field for the user to jot down observations
- **Metronome**: Embedded metronome widget (tap tempo, BPM slider)
- **Backing tracks**: Links to or embedded YouTube backing tracks per phase
- **Spaced repetition**: Review prompts for material from earlier phases
- **Dark mode**: Toggle for low-light practice sessions
- **Export/import progress**: JSON backup/restore for progress data
- **Notification reminders**: Push notifications for daily practice (requires more PWA setup)

## How to Run Locally

The app needs to be served over HTTP (not file://) for the service worker to function:

```bash
# Python
cd guitar-app && python3 -m http.server 8000

# Node
npx serve guitar-app

# VS Code
# Use the "Live Server" extension — right-click index.html → "Open with Live Server"
```

Then visit `http://localhost:8000` and use your browser's "Install" or "Add to Home Screen" option.

## How to Install as an App

### iPhone / iPad
1. Open in Safari (must be Safari, not Chrome)
2. Tap the Share button (square with arrow)
3. Scroll down and tap "Add to Home Screen"
4. Name it and tap "Add"

### Android
1. Open in Chrome
2. Tap the three-dot menu
3. Tap "Install app" or "Add to Home Screen"

### Desktop (Chrome/Edge)
1. Look for the install icon in the address bar
2. Or go to Menu → "Install Guitar Roadmap..."

## Conversation History

This project was built through an iterative conversation in Claude.ai. Key decisions:
- User specifically requested a building-block approach (each week builds on the last)
- 15–30 min daily practice structure (5 warmup / 15 focus / 10 free play)
- Bluegrass is the target genre but general theory comes first
- Resources were web-searched and curated for quality — preference for free content with subscription options noted
- The HTML version was created first as a static document, then converted to this PWA
- Gibson App integration was explored but their lesson catalog isn't publicly available — left as a future enhancement
- A markdown version of the plan also exists (Guitar_Learning_Plan.md) from the earlier iteration
