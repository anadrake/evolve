# EVOLVE — High Level Design Document
**Version:** 0.7
**Status:** Prototype-ready
**Last Updated:** May 2026

---

## CHANGELOG

**v0.7 (this session)**
- **Prototype blockers closed.** Board, countdown, illustration approach, branch cadence, and stat math all locked.
- Board: **square 4-neighbor grid, unbounded, random next tile** (Dorf-faithful infinite plane).
- Threat horizon: **base 6 turns, severity-scaled, late-game floor of 1.**
- Species card (prototype only): **text-only** — name + stat readout + trait blurbs. Illustration pipeline deferred.
- Branch cadence: **Trait Moment ~7 turns, Stat Unlock ~20 turns.**
- Tile → stat mapping, stat formulas, and threat severity ranges defined (see STAT SYSTEM / THREAT SYSTEM).

**v0.6 (prior session)**
- Helix visualization **cut**. Species card and chronicle absorb its jobs.
- **Metabolism replaced with Adaptability.** New stat trio: Vitality (shield), Fertility (pool), Adaptability (regen).
- Tier gating moves from Fertility to Adaptability.
- Threat-stat relationship reframed: no threat is immune-able; stats determine *how* you experience the hit.

**v0.5**
- Pacing: horizon/countdown threat model replaces per-turn threat resolution.
- Resource layer collapsed. **Tile placement is the genome** — no separate Biomass spend step.
- Species card and Chronicle locked as persistent surfaces.

---

## PILLARS

1. **Guide, don't control**
   You shape the environment, the species surprises you. The player is a gardener, not a god.

2. **Survival is earned, not given**
   The world pushes back. Every adaptation is a response to pressure.

3. **Every run is a story worth telling**
   Each playthrough produces a unique species with a shareable identity. The reward is discovery: *"I made a psychic mech-building octopus and I need to tell someone."*

4. **Each run makes the next run richer**
   You never start from zero twice. Legacy points unlock new evolutionary possibilities that push the ceiling higher each time you play.

---

## CONTENT PILLAR

**"Rooted in Earth, branching into the impossible"**
Evolution follows recognizable patterns early, then surprises you with things that could never exist. The player feels grounded then delighted. Weirdness is earned by surviving long enough to reach it.

---

## EXTINCTION CONDITION

- Population meter hits zero
- Player always has a response window before extinction
- Runs are unlimited — pressure scales infinitely with run length
- Longer runs = higher ceiling, stronger personal best moments

### Difficulty Scaling
```
Early game  → gentle pressure, learning the system, discovering first stats
Mid game    → events hit harder, advanced stats matter more, legacy unlocks kick in
Late game   → existential-level events (mass extinction, ice age, asteroid)
Endgame     → civilizational-tier events only beatable with top-tier unlocks
               (space travel isn't just a label, it's how you survive the asteroid)
```

---

## CORE LOOP

**Tile placement is the genome.** Each tile placed directly encodes part of the species' stat profile. There is no separate resource currency or spend step — the board *is* the species.

Target feel: Dorf Romantik. Hypnotic, tactile, spatially satisfying.

### Board
- **Grid:** square, 4-neighbor (cardinal adjacency)
- **Bounds:** unbounded; board expands as the player places tiles
- **Tile supply:** one random tile at a time, no choice
- **Camera:** pan/zoom required from day one (consequence of unbounded board)

### Loop Steps
```
1. THREAT HORIZON    → upcoming threat type revealed with countdown
2. PLACEMENT TURNS   → player places tiles freely across the countdown
3. RESOLUTION        → threat severity rolled, damage applied based on stats
4. CHRONICLE BEAT    → outcome appended to species' running story
```

**Key design notes:**
- Player sees *what* threat is coming and roughly *when*, but not *how hard* it hits.
- Several uninterrupted placement turns before each resolution, not threat-per-turn.
- Branch events fire between turns based on placement patterns.
- Because the board is unbounded, the meaningful tradeoff in placement is **adjacency optimization** (which neighbors to match for which stat), not real-estate scarcity. If the prototype feels directionless, threat pressure is the lever to pull — not board space.

This produces four satisfying outcomes per resolution:
- Prepared + lucky → easy round
- Prepared + unlucky → survived but hurt
- Unprepared + lucky → got away with it
- Unprepared + unlucky → genuine danger

---

## TILE TYPES

### Prototype
| Tile | Category | Stat Generated |
|---|---|---|
| Ocean | Water | Fertility |
| Shallow | Water | Adaptability |
| Hydrothermal | Water | Vitality |
| Shore | Land | Adaptability |
| Forest | Land | Fertility |
| Desert | Land | Vitality |

### Stat Generation Rule
- Each placed tile contributes **+1 to its mapped stat**.
- **+1 bonus per same-type adjacent neighbor** (max +4 on 4-neighbor grid).
- Clustering is rewarded; isolated tiles still contribute base stat.

Each species path has a different stat fingerprint:
- Water-heavy runs: Fertility + Adaptability lean (with Vitality possible via Hydrothermal)
- Land-heavy runs: Fertility + Vitality lean (with Adaptability via Shore)
- Mixed runs: balanced stat spread

### Later Unlocks (via Meta Progression)
- Sky
- Subterranean
- ??? (deep progression rewards)

---

## SPECIES PATHS

| Path | Tile Affinity | Prototype |
|---|---|---|
| Aquatic | Water tiles | ✓ |
| Terrestrial | Land tiles | ✓ |
| Aerial | Sky tiles | Later |
| Subterranean | Underground tiles | Later |

Early tile choices subtly lock the player into a species identity and evolutionary path, feeding Pillar 3.

---

## STAT SYSTEM

Three starting stats, all operating on the population layer. Each has a distinct verb and a direct relationship to threat experience.

### The Health Bar Metaphor

```
Fertility    → size of the bar      (how much health you have)
Vitality     → shield over the bar  (absorbs incoming damage)
Adaptability → regen on the bar     (recovers lost health over time)
```

### Stats Detail & Formulas

| Stat | Verb | Formula |
|---|---|---|
| **Fertility** | *Expand* | Max population = **50 + (Fertility × 5)**. Fertility=10 → max 100 (default). |
| **Vitality** | *Block* | Each point absorbs **1 damage** per threat resolution. Vitality=10 reduces a 35-damage threat to 25 damage. |
| **Adaptability** | *Regenerate* | Regen per turn = **Adaptability × tier_factor**, where tier_factor is 0.3 / 0.2 / 0.1 / 0 by stability tier. Adaptability=10 in THRIVING = +3/turn. |

### Starting Values
All stats start at **0 / 0 / 0**. Species begins as a blank slate. Stats accrue purely from tile placement — this *is* the evolution.

### The Tradeoff Triangle
```
Vitality      → don't take the hit
Fertility     → have more to lose
Adaptability  → recover what's lost
```

No single stat does everything. Each occupies a temporal niche:
- **Fertility = setup** (matters before things go wrong)
- **Adaptability = steady-state** (matters during sustained pressure)
- **Vitality = crisis** (matters when things are dire)

---

## THREAT SYSTEM

### Horizon Model
Threats announce their **type** in advance with a countdown. Severity remains hidden until resolution.

### Countdown Rules
- **Base countdown (early game):** 6 placement turns
- **Severity-scaled:** bigger threats announce earlier. Mass-extinction-tier events may give 10+ turns. Minor threats may give 3.
- **Late-game floor:** 1 placement turn (creates panic moments for endgame threats). Doesn't violate the "always a response window" rule — player still gets one full placement turn before resolution.

### Three Categories (Prototype)
**No threat is immune-able.** Every threat damages the population. Stats determine *how* the player experiences the hit, not whether it lands.

| Category | Damage Shape | Best Countered By |
|---|---|---|
| **Predatory** | Sudden spike — full damage on resolution turn | Vitality (deflect the hit) |
| **Epidemic** | Sustained drain — damage spread evenly across countdown turns | Fertility (bigger pool buys time) |
| **Environmental** | Grinding pressure — damage applied during placement turns leading up to resolution | Adaptability (heal through it) |

### Threat Anatomy
```
Type      → which category (revealed on horizon)
Countdown → number of placement turns before resolution
Severity  → how hard it hits (hidden until resolution)
```

### Severity Ranges
- **Early game:** 10–25 damage
- **Mid game:** 25–50 damage
- **Late game:** 50–90 damage (existential tier)

Severity scales with run length, not on a fixed cycle.

### Example Threats by Category

**Predatory (spike damage)**
```
Early:  Apex Predator Emerges, Parasite Outbreak, Territory Invasion
Mid:    Pack Hunters, Venomous Competitor
Late:   Psychic Predator, Crystalline Plague
```

**Epidemic (sustained drain)**
```
Early:  Disease Outbreak, Genetic Bottleneck, Nesting Disruption
Mid:    Mutagenic Spore, Sterility Pathogen
Late:   Memory Virus, Entropy Cascade
```

**Environmental (grinding pressure)**
```
Early:  Drought, Volcanic Winter, Ocean Acidification
Mid:    Toxic Bloom, Resource Collapse
Late:   Gravity Shift, Dimensional Leak
```

*Late/weird threats only appear after sufficient meta progression unlocks. Weirdness is earned.*

---

## SPECIES IDENTITY SYSTEM

Species identity emerges automatically from player choices — no direct input required. Discovery IS the reward.

### Identity Sources

**Source 1: Stat Profile**
```
High Vitality      → heavily armored, robust creatures
High Fertility     → swarming, colonial, numerous creatures
High Adaptability  → resilient, regenerative, plastic creatures
```

**Source 2: Tile Path**
```
Water heavy → aquatic, fluid, tentacled forms
Land heavy  → grounded, limbed, territorial forms
Mixed       → amphibious, transitional, weird hybrids
```

Species identity = intersection of stat profile + tile path.

### Species Card (Prototype Format)
A persistent panel showing:
- **Auto-generated species name**
- **Stat readout** (current Vitality / Fertility / Adaptability values)
- **Trait blurbs** accumulated from branch events

**No illustration in the prototype.** This is intentional — the card has to carry the share-worthy moment on language alone. If it does, language is doing the work and the art pipeline question can wait. If it doesn't, illustration becomes the next priority.

**Load-bearing systems for the prototype card:**
- Procedural name generator (must produce names that feel evocative, not random-noun-soup)
- Trait blurb library / template system (must read like creature lore, not stat dumps)

The species card is the primary shareable artifact — what players screenshot and send to friends.

### Chronicle
A running story log appending narrative beats from threat events and branch choices. Each run becomes a biography. Supports the "unique shareable stories" pillar.

### Branch Events

Two distinct event types fire at different rates during a run:

**Branch Event Type 1: Trait Moment (flavor)**
- Cadence: roughly every **7 turns**
- Purely cosmetic, fires more frequently
- Player chooses from 2-3 visual/descriptive options
- No mechanical impact, shapes species identity and story
```
"Your species is developing, choose one:
→ Bioluminescence
→ Chitinous plating
→ Translucent skin"
```

**Branch Event Type 2: Stat Unlock (mechanical)**
- Cadence: roughly every **20 turns**
- Rarer, bigger moment, changes the loop
- Introduces a new upgradeable stat
- Meaningful mechanical expansion
```
"Your species is developing neural complexity.
→ Neural Cluster unlocked (Intelligence stat added)"
```

**Watch in playtest:** with this slower cadence, the first ~15 turns may feel under-eventful. If so, tighten Trait Moments to ~5 before tightening Stat Unlocks.

---

## POPULATION SYSTEM

Three models combined into one cohesive mechanic.

### The Bar
Population is a health bar from 0 to Fertility-determined max (formula in STAT SYSTEM). Simple, readable, easy to tune.

### Population Count
The bar is represented as actual organism count — damage feels biological, not abstract.
```
100 = 10,000 individuals
50  = 5,000 individuals
10  = 1,000 individuals
```
*"I lost 2,000 individuals to that epidemic" hits harder than "I lost 10 HP."*

### Stability Tiers
Thresholds on the same bar give players an at-a-glance status read.
```
100-75  → THRIVING    (green)
74-50   → STABLE      (yellow)
49-25   → STRUGGLING  (orange)
24-1    → CRITICAL    (red)
0       → EXTINCT
```

*Note: thresholds are expressed as % of max pop, not absolute, so they scale with Fertility.*

### Adaptability Interaction
Stability tier directly gates how much Adaptability can recover each turn. Tier factors applied to Adaptability stat:
```
THRIVING   → Adaptability × 0.3 per turn
STABLE     → Adaptability × 0.2 per turn
STRUGGLING → Adaptability × 0.1 per turn
CRITICAL   → Adaptability × 0.0 (too stressed to recover)
```

**Key design note:** Once CRITICAL is reached, Adaptability stops saving you. Only Vitality (block the next hit) can dig you out. Fertility's value was already front-loaded as max ceiling. This creates genuine panic in the endgame and gives each stat a clear temporal niche.

### Tier Events (future consideration)
Tiers could trigger special events — e.g. hitting CRITICAL unlocks a desperation mechanic or fires a warning event giving the player one last window to recover.

---

## META PROGRESSION

Connects runs together. Funds Pillar 4 — each run makes the next run richer. **Deferred from prototype scope** — meta progression mechanics are intentionally left undecided until the single-run loop is validated.

### Pillar 1: Stat Unlocks
Unlocks more advanced stats available at later evolution levels in future runs.

```
Always available:  Vitality, Fertility, Adaptability
Mid unlocks:       Intelligence, Sensitivity
Late unlocks:      Collective Consciousness, Technological Aptitude
Endgame:           Telepathy, Space Travel
```

*Late unlocks aren't just prestige — they're the only tools that can handle late-game threats.*

### Pillar 2: Tile Unlocks
Unlocks new terrain types available in future runs.

```
Always available:  Water, Land
Mid unlocks:       Sky, Subterranean
Late unlocks:      ??? (earned through deep progression)
```

**Together:**
```
Tile Unlocks  → expand where your species can go
Stat Unlocks  → expand what your species can become
Both together → each run has a higher ceiling than the last
```

### Parked Meta-Unlocks
- **"Come back stronger"** — Adaptability meta-unlock that allows the species to gain permanent stat bumps after surviving severe threats. Reserved for later; baseline Adaptability stays as pure regen.

---

## OPEN CONSIDERATIONS (for after prototype playtest)

### Meta Progression (deferred — needs working single-run loop first)
**1. Legacy Points currency model**
- Single currency earned every run, OR
- Separate currencies per species path (aquatic runs earn ocean points, etc.), OR
- Milestone based — just reach X evolution level

**2. Unlock order: linear vs. player-chosen**
- Linear → simpler, guided progression
- Player choice → more replayability, different strategies per player

### Art Pipeline (deferred — text-only species card first)
**3. Species card illustration generation method**
How are creature illustrations actually produced? Procedural composition, AI generation, hand-drawn library, hybrid? Major art pipeline decision. Only revisit if text-only card fails the "I have to tell someone" test in playtest.

### Polish & Feel
**4. Tile placement visuals**
Animation, sound, neighbor reactions on placement. Drives the Dorf Romantik feel target. Prototype this after the loop works; iterate to taste.

### Shelved (revisit later)
**5. Tile/Threat environmental interaction**
Certain tile configurations could affect threat vulnerability (e.g. ice age punishes low Vitality, mitigated by warm tiles). Shelved until prototype loop is proven.

**6. Compound multi-stat threats**
Threats that punish more than one low stat. Shelved for prototype simplicity.

**7. "Make Fertility more playable mid-run"**
Fertility is currently set early and goes static. Watch in playtest — if it feels passive, add active mid-run interactions (tile-based pool growth, branch events that expand it). Don't pre-solve.

---

## CUT FROM PRIOR VERSIONS

- **Helix visualization** (v0.6) — species card and chronicle cover its jobs more directly. Diagrams aren't shareable; creatures are.
- **Biomass / single resource layer** (v0.5) — collapsed into tile placement directly. The board is the genome.
- **Metabolism stat** (v0.6) — replaced by Adaptability. Operated on a different (resource) layer than Vitality and Fertility, which created an asymmetry. Adaptability is on the population layer alongside the others, with a clean active verb.

---

## PROTOTYPE SCOPE (v0.7 baseline)

Everything below is locked enough to start building:

- **Board:** square 4-neighbor, unbounded, random tile draw, pan/zoom camera
- **Tiles:** 6 prototype types with defined stat mappings
- **Stats:** 3 starting (Vitality, Fertility, Adaptability) with formulas
- **Population:** 0 to (50 + Fertility × 5), 4 stability tiers as % of max
- **Threats:** 3 categories, horizon countdown base 6 (severity-scaled, min 1), severity 10–90 by phase
- **Branch events:** Trait ~7 turns, Stat Unlock ~20 turns
- **Species card:** text-only — name + stats + trait blurbs
- **Chronicle:** running log of resolutions and branch choices

Out of prototype scope: meta progression, illustration pipeline, animation polish, sound.

---

*Next session: pick up from prototype implementation — likely starting with board + tile placement, then stat accumulation, then a single threat resolution.*
