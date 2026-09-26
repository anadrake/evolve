# EVOLVE — Prototype Plan
**Companion to:** EVOLVE_DESIGN_v0_8.md
**Status:** Active
**Last Updated:** September 2026

---

## APPROACH

Lightweight web prototypes first, then Swift/SpriteKit.

- **Web protos answer: is the loop interesting?** Edge-matching, queue size, deck ratios, threat pacing, stat tradeoffs.
- **Swift build answers: does it feel good?** Haptics, placement animation, the Dorf Romantik snap.

A web proto passing means the system is worth building properly. It does not mean the game works — feel is validated in Swift.

### Ground rules
- **Keep web protos ugly on purpose.** Colored squares and text. No art, no juice, no polish.
- **Web code is disposable.** Don't port code to Swift. Port the tuning config and the learnings.
- **All tuning values live in one config object** from day one, so they map straight to Swift.
- **One question per proto.** If a feature doesn't help answer that proto's question, it waits.

---

## PHASE 1 — WEB PROTO 1: PLACEMENT TOY

**Question:** Is placing tiles fun on its own?

### Build
- Square 4-neighbor grid, unbounded, with basic pan/zoom (drag to pan, pinch/scroll to zoom)
- First tile placed at origin; subsequent tiles must be adjacent to an existing tile *(proto assumption)*
- Tile render: center square = primary biome color, 4 edge triangles = edge sub-biome colors
- See-ahead queue of 3: front tile is playable, next 2 visible
- Tap to rotate the front tile before placing *(proto assumption — test with and without)*
- Valid placement spots highlighted; matching edges highlighted on hover/preview
- Edge-matching scoring feeds the primary biome's stat
- Live readout: Fertility / Vitality / Adaptability, turn count, last placement score
- Deck generator using composition ratios from config

### Don't build yet
Population, threats, tiers, branch events, species card, name generator, save/load, menus, sound, animation.

### Exit criteria
- Placement creates real decisions (not "obviously this spot" every turn)
- Rotation question answered: required, optional, or cut
- Queue size feels right (try 2, 3, 4)
- Deck ratios produce variety without frustration
- Stats end up meaningfully different across runs depending on choices

---

## PHASE 2 — WEB PROTO 2: SURVIVAL LOOP

**Question:** Do the three stats create real tradeoffs under pressure?

### Build (on top of Proto 1)
- Population bar with max = formula from config
- Four stability tiers with color readout
- Adaptability regen per turn, tier-gated
- Vitality absorption on threat damage
- Threat horizon: one threat visible at a time with countdown
- Start with Predatory only (simplest shape: spike on resolution), then add Epidemic and Environmental
- Severity rolled at resolution from the range for current game stage
- Extinction at population 0; restart button; show turns survived

### Don't build yet
Branch events, species card, name generator, chronicle, meta progression, weird late-game threats.

### Exit criteria
- Each threat type pushes toward a different stat
- The "prepared + unlucky / unprepared + lucky" outcomes happen and feel distinct
- CRITICAL tier creates panic, not hopelessness
- Runs last long enough to be interesting, short enough to replay
- Fertility passivity: does it feel inert mid-run? (v0.6 watch item)

---

## PHASE 3 — WEB PROTO 3 (OPTIONAL): IDENTITY LAYER

**Question:** Does the run produce a species worth talking about?

Only if Proto 2 feels right and there's appetite before moving to Swift.

- Trait Moments (~7 turns) and Stat Unlocks (~20 turns)
- Text-only species card: generated name + stat readout + trait blurbs
- Watch: do the first 15 turns feel under-eventful?

Can also be skipped and built directly in Swift.

---

## PHASE 4 — SWIFT / SPRITEKIT BUILD

**Question:** Does it feel like Dorf Romantik?

- Port the tuned config from the web protos
- Rebuild placement with touch, haptics, and placement feedback
- Pan/zoom camera from day one
- Bring in Proto 2 loop once placement feels good in hand

---

## CONFIG (v0.8 starting values)

Starting points to playtest, not sacred.

```js
const CONFIG = {
  // Tiles
  biomes: ["Ocean", "Shallow", "Hydrothermal", "Shore", "Forest", "Desert"],
  biomeStat: {
    Ocean: "fertility",       Forest: "fertility",
    Hydrothermal: "vitality", Desert: "vitality",
    Shallow: "adaptability",  Shore: "adaptability",
  },
  // Split/specialty tiles pull edges from a related biome.
  // PLACEHOLDER pairings for proto — not in v0.8, tune freely.
  relatedBiomes: {
    Ocean: ["Shallow", "Hydrothermal"],
    Shallow: ["Ocean", "Shore"],
    Hydrothermal: ["Ocean", "Desert"],
    Shore: ["Shallow", "Forest"],
    Forest: ["Shore", "Desert"],
    Desert: ["Forest", "Hydrothermal"],
  },
  deckComposition: { pure: 0.50, split: 0.30, specialty: 0.15, chaos: 0.05 },
  queueSize: 3,

  // Scoring
  basePlacementScore: 1,   // +1 to primary stat for placing
  perMatchedEdge: 1,       // +1 per matched edge (max +4)
  unmatchedEdgePenalty: 0,

  // Stats
  startingStats: { fertility: 0, vitality: 0, adaptability: 0 },
  basePopulation: 50,
  popPerFertility: 5,      // maxPop = 50 + fertility * 5
  vitalityAbsorbPerPoint: 1,
  regenTierFactor: { THRIVING: 0.3, STABLE: 0.2, STRUGGLING: 0.1, CRITICAL: 0 },

  // Tiers (% of max population)
  tiers: { THRIVING: 75, STABLE: 50, STRUGGLING: 25, CRITICAL: 1 },

  // Threats
  horizonBaseTurns: 6,     // severity-scaled; bigger threats announce earlier
  horizonMinTurns: 1,      // late-game floor
  severity: {
    early: [10, 25],
    mid:   [25, 50],
    late:  [50, 90],
  },
  // Damage shapes:
  // predatory     = spike on resolution
  // epidemic      = drain spread across the countdown
  // environmental = grinding pressure on placement turns

  // Branch events (Proto 3)
  traitMomentEvery: 7,
  statUnlockEvery: 20,
};
```

---

## PROTO ASSUMPTIONS (not in v0.8 — decide via playtest)

1. **Adjacency rule** — must new tiles touch an existing tile? Proto default: yes.
2. **Rotation** — can the player rotate the front tile? Proto default: yes, tap to rotate. Big impact on how hard edge-matching is.
3. **Related biome pairings** — which biomes appear together on split/specialty tiles. Placeholder table above.
4. **Game stage thresholds** — at what turn do severity ranges shift early → mid → late. Proto default: turn 20 / turn 50, tune.
5. **Epidemic vs. Environmental distinction** — both are multi-turn damage; confirm they feel different in play.

Roll resolved assumptions back into the next design doc version.

---

## TESTING ON PHONE

### Web protos
- **Same Wi-Fi, live iteration:** run a local server bound to your network (e.g. `python3 -m http.server 8000`), then open `http://<mac-local-ip>:8000` in Safari on the phone.
- **Shareable link:** push to GitHub and enable GitHub Pages on the repo or a `/prototypes/web` folder. Gives Daniel a URL.
- **Full-screen feel:** in Safari, Share → Add to Home Screen. Add viewport and `apple-mobile-web-app-capable` meta tags so it launches without browser chrome.
- **Debugging:** iPhone Settings → Safari → Advanced → Web Inspector on; Mac Safari → Develop menu → select the phone.

### Swift app
- **Free Apple ID:** plug the phone into the Mac, sign in to Xcode with your Apple ID (Personal Team), select the phone as run target, Run. Enable Developer Mode on the phone when prompted. Builds expire after about 7 days; fine for solo testing.
- **Wireless:** after the first wired run, Xcode can deploy over Wi-Fi.
- **Paid Apple Developer Program:** needed for TestFlight (sending builds to Daniel) and longer-lived installs. Defer until the Swift build is worth sharing.

---

## SUGGESTED REPO LAYOUT

```
evolve/
  EvolveV1/
    EVOLVE_DESIGN_v0_8.md
    PROTOTYPE_PLAN.md          ← this file
  prototypes/
    web/
      proto1-placement/
        index.html             ← single file, vanilla JS + canvas, no build step
      proto2-survival/
        index.html
```

Vanilla HTML/JS/canvas, single file per proto, no framework or build step. Keeps it disposable, easy for Claude Code to iterate on, and deployable to GitHub Pages as-is.
