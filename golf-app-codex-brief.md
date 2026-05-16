# Golf Scorecard App — Codex Build Brief

## Project Overview
Mobile-first golf scorecard app. Orange & grey color scheme. Designed for one-handed use on the course.

---

## Color System

| Token | Hex | Usage |
|---|---|---|
| Orange Primary | `#FF7A00` | CTAs, active states, birdie/eagle indicators |
| Orange Muted | `#E87722` | Headers, scorecard column titles |
| Orange Soft | `#FF9A45` | Dark-mode accent, glass overlays |
| Dark BG | `#111111` | Dark Premium background |
| Card Dark | `#1A1A1A` | Dark card surfaces |
| Grey Text | `#888888` | Secondary labels, bogey symbols |
| Light BG | `#F5F5F5` | Light mode background |
| White Card | `#FFFFFF` | Light mode cards |
| Border | `#222222` (dark) / `#DDDDDD` (light) | Dividers |

---

## Scoring Symbols System

These are standard golf scorecard markings — implement as bordered containers around the score number:

| Result | Score vs Par | Symbol Shape | Color |
|---|---|---|---|
| Albatross | −3 | Triple circle (outer ring) | Orange |
| Eagle | −2 | Double circle (circle + outer ring) | Orange `#FF7A00` |
| Birdie | −1 | Single circle | Orange `#FF7A00` |
| Par | E | No border / plain | Default text color |
| Bogey | +1 | Single square border | Grey `#888888` |
| Double Bogey | +2 | Double square (square + outer square) | Grey `#888888` |
| Triple Bogey+ | +3+ | Filled square | Dark grey / `#555555` bg |

### Implementation (CSS approach)
```css
/* Birdie */
.score-birdie {
  border: 2px solid #FF7A00;
  border-radius: 50%;
  color: #FF7A00;
}

/* Eagle */
.score-eagle {
  border: 2px solid #FF7A00;
  border-radius: 50%;
  outline: 2px solid #FF7A00;
  outline-offset: 3px;
  color: #FF7A00;
}

/* Bogey */
.score-bogey {
  border: 2px solid #888888;
  border-radius: 0;
  color: #888888;
}

/* Double Bogey */
.score-double-bogey {
  border: 2px solid #888888;
  border-radius: 0;
  outline: 2px solid #888888;
  outline-offset: 3px;
  color: #888888;
}

/* Triple Bogey+ */
.score-triple-bogey {
  background: #555555;
  color: #cccccc;
  border-radius: 2px;
}
```

---

## 5 Design Directions (pick one to build)

### Direction 1 — Dark Premium / Tour Pro
> Best for: competitive golfers, premium feel, high legibility outdoors

**Layout:** Single hole per screen. Vertical layout. Big centered score number. Minus/Plus buttons flanking. Stats row beneath.

**Colors:** `#111` base, `#FF7A00` accent only. `#888` secondary text.

**Typography:** Heavy sans-serif (SF Pro Display / Montserrat Bold) for score. Tight monospace for stats.

**Key screens:**
- Hole view: Hole #, Par, Distance, Handicap at top → Giant score number center → +/− buttons → Stats row (Putts, GIR, FIR) → Running total bar at bottom
- Scorecard summary: Dark table, orange row for current player, symbol-annotated scores
- Leaderboard: Card list, orange highlight for #1 position

**Scoring symbols:** Orange circles (birdie/eagle), grey squares (bogey+) on dark card backgrounds.

---

### Direction 2 — Scorecard Grid / Classic Reimagined
> Best for: traditional golfers, group rounds, data completeness

**Layout:** Scrollable table. Front 9 / Back 9 sections. Each row = one hole. Swipe to see other players.

**Colors:** `#F5F5F5` background, `#E87722` headers, white cells, alternating grey row tint.

**Typography:** Roboto Condensed or similar — tight, numeric, every column fits on screen.

**Columns:** Hole | Par | HCP | Score | +/−

**Key screens:**
- Full 18-hole grid with symbol annotations inline
- Tap a score cell to edit → modal popover with +/− input
- Totals row pinned at bottom (OUT / IN / TOTAL)
- Player switcher tabs at top

**Scoring symbols:** Rendered directly in the score cell — orange circle text for birdie, grey square border for bogey.

---

### Direction 3 — Sports Dashboard / Data-Rich
> Best for: stat-tracking golfers, multiple players, performance analysis

**Layout:** Top banner = live score vs par (big orange number). Middle = recent holes as mini tiles. Bottom = active hole input.

**Colors:** White cards on `#EFEFEF` background. `#FF6B00` for positive performance. Grey for neutral/over par.

**Typography:** Bold display for score vs par. Regular weight for stats. Clear hierarchy.

**Key screens:**
- Dashboard: Score summary → Recent holes strip → Active hole card (full-width orange bg)
- Stats view: Birdies, bogeys, GIR%, fairways hit, putts per hole as mini metric cards
- Round history list

**Scoring symbols:** Color-coded tile backgrounds — orange tint for birdie tiles, grey tint for bogey tiles.

---

### Direction 4 — Frosted Glass / Course Lifestyle
> Best for: casual rounds, scenic courses, premium brand feel

**Layout:** Full-bleed course photo background (blurred). Frosted glass panels floating over. Swipe left/right between holes.

**Colors:** Course green photo bg. `rgba(255,255,255,0.12)` glass panels. `#FF9A45` orange accent. White text with drop shadow.

**Typography:** Thin/light weight headline. Medium weight score. SF Pro or similar native font.

**Key screens:**
- Hole view: Swipeable full-screen card per hole, glass stats panel at bottom
- Score input: Tap score number → popover with slider or +/− 
- End-of-round: Glass summary card over blurred scorecard photo

**Scoring symbols:** Glowing colored ring effects — orange glow circle (birdie), translucent grey square (bogey).

---

### Direction 5 — Bold Flat / Game Energy
> Best for: casual/social rounds, quick scoring, fun gamification

**Layout:** Chunky UI. Entire screen = one hole. Giant score number in an orange circle. Oversized +/− buttons. Stats bar at bottom.

**Colors:** `#FF7A00` header bar, `#F7F7F7` body, white score circle outline, `#EEE` button backgrounds.

**Typography:** Extra-bold rounded sans-serif (Nunito ExtraBold / Poppins Black). Big. Thumb-friendly.

**Key screens:**
- Hole input: Full screen, one interaction (tap + or −)
- Running totals bar: Eagles / Birdies / Pars / Bogeys count as orange pill badges
- Summary screen: Colorful badge-style results per hole, shareable layout

**Scoring symbols:** Large badge circles/squares — orange filled circle for birdie, grey filled square for bogey, with label text underneath.

---

## Core App Screens (all directions)

1. **Home / New Round** — Start round, choose course, select players, set tees
2. **Hole View** — Score entry for current hole (main loop screen)
3. **Scorecard** — Full 18-hole grid, all players, with scoring symbols
4. **Stats / Dashboard** — Performance metrics for the round
5. **End of Round** — Summary, save, share
6. **Settings** — Handicap, preferred tees, display options

---

## Data Model

```typescript
type Round = {
  id: string
  date: string
  course: string
  players: Player[]
  holes: HoleScore[]
  completed: boolean
}

type Player = {
  id: string
  name: string
  handicap: number
}

type HoleScore = {
  hole: number          // 1–18
  par: number           // 3, 4, or 5
  distance: number      // yards
  handicap: number      // stroke index 1–18
  score: number         // strokes taken
  putts?: number
  gir?: boolean         // green in regulation
  fir?: boolean         // fairway in regulation
}

type ScoreResult = 
  | 'albatross'         // −3
  | 'eagle'             // −2
  | 'birdie'            // −1
  | 'par'               // E
  | 'bogey'             // +1
  | 'double-bogey'      // +2
  | 'triple-bogey-plus' // +3 or worse
```

---

## Tech Stack Recommendations

- **React Native** (cross-platform iOS + Android) or **Flutter**
- **State:** Zustand or Redux Toolkit
- **Storage:** AsyncStorage for local rounds, optional Supabase/Firebase for cloud sync
- **Navigation:** React Navigation (tab + stack)
- **Charts:** Victory Native or Recharts (for stats screen)

---

## Notes from Design Research

- One-handed usability is critical — all tap targets should be 44px+ (Apple HIG minimum)
- Outdoor readability: high contrast, avoid light grey on white
- Orange accent on dark background is the highest-contrast combo for sunlight use
- Keep score entry to 1–2 taps max per hole (the round is 18 holes — friction compounds)
- Golf Pad, Hole19, and 18Birdies are the main competitors — study their scorecard UX
- Traditional golfers expect the physical scorecard symbols (circles/squares) — don't reinvent them

---

*Generated via Cowork + Anthropic Claude · May 2026*
