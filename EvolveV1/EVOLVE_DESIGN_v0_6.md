# EVOLVE — High Level Design Document
**Version:** 0.6
**Status:** In Progress
**Last Updated:** April 2026

---

## CHANGELOG

**v0.6 (this session)**
- Helix visualization **cut**. Species card and chronicle absorb its jobs.
- **Metabolism replaced with Adaptability.** New stat trio: Vitality (shield), Fertility (pool), Adaptability (regen).
- Tier gating moves from Fertility to Adaptability.
- Threat-stat relationship reframed: no threat is immune-able; stats determine *how* you experience the hit.
- "Come back stronger" parked as a meta-progression unlock on Adaptability.

**v0.5 (prior session)**
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

This produces four satisfying outcomes per resolution:
- Prepared + lucky → easy round
- Prepared + unlucky → survived but hurt
- Unprepared + lucky → got away with it
- Unprepared + unlucky → genuine danger

---

## TILE TYPES

### Prototype
| Category | Tiles |
|---|---|
| Water | Ocean, Shallow, Hydrothermal |
| Land | Shore, Forest, Desert |

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

### Stats Detail

| Stat | Verb | Effect |
|---|---|---|
| **Vitality** | *Block* | Shield over the population bar. Reduces damage taken on threat resolution. |
| **Fertility** | *Expand* | Sets the maximum population ceiling. Static once established — bigger pool, more to lose. |
| **Adaptability** | *Regenerate* | Recovers population over time. Gated by stability tier. |

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

### Three Categories (Prototype)
**No threat is immune-able.** Every threat damages the population. Stats determine *how* the player experiences the hit, not whether it lands.

| Category | Damage Shape | Best Countered By |
|---|---|---|
| **Predatory** | Sudden spike | Vitality (deflect the hit) |
| **Epidemic** | Sustained drain over the horizon | Fertility (bigger pool buys time) |
| **Environmental** | Grinding pressure across turns | Adaptability (heal through it) |

### Threat Anatomy
```
Type      → which category (revealed on horizon)
Countdown → number of placement turns before resolution
Severity  → how hard it hits (hidden until resolution)
```

### Threat Severity Scaling
- Scales with run length, not on a fixed cycle
- Early game: forgiving rolls
- Late game: punishing rolls, existential stakes

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

### Species Card
A persistent panel showing:
- Creature illustration (auto-generated)
- Auto-generated species name
- Trait blurbs accumulated over the run

The species card is the primary shareable artifact — what players screenshot and send to friends.

### Chronicle
A running story log appending narrative beats from threat events and branch choices. Each run becomes a biography. Supports the "unique shareable stories" pillar.

### Branch Events

Two distinct event types fire at different rates during a run:

**Branch Event Type 1: Trait Moment (flavor)**
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
- Rarer, bigger moment, changes the loop
- Introduces a new upgradeable stat
- Meaningful mechanical expansion
```
"Your species is developing neural complexity.
→ Neural Cluster unlocked (Intelligence stat added)"
```

---

## POPULATION SYSTEM

Three models combined into one cohesive mechanic.

### The Bar
Population is a health bar from 0 to (Fertility-determined max). Simple, readable, easy to tune. Default max = 100.

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

### Adaptability Interaction
Stability tier directly gates how much Adaptability can recover each turn:
```
THRIVING   → Adaptability regenerates +3 population per turn
STABLE     → Adaptability regenerates +2 population per turn
STRUGGLING → Adaptability regenerates +1 population per turn
CRITICAL   → Adaptability regenerates +0 (too stressed to recover)
```

**Key design note:** Once CRITICAL is reached, Adaptability stops saving you. Only Vitality (block the next hit) can dig you out. Fertility's value was already front-loaded as max ceiling. This creates genuine panic in the endgame and gives each stat a clear temporal niche.

### Tier Events (future consideration)
Tiers could trigger special events — e.g. hitting CRITICAL unlocks a desperation mechanic or fires a warning event giving the player one last window to recover.

---

## META PROGRESSION

Connects runs together. Funds Pillar 4 — each run makes the next run richer.

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

## OPEN CONSIDERATIONS (for next session)

### Tuning
**1. Countdown length for horizon threats**
How many placement turns between threat reveal and resolution? Likely scales with severity. Needs prototype playtest.

**2. Branch event firing ratios**
Trait Moments vs. Stat Unlocks. Rough starting point: trait moment every ~5 turns, stat unlock every ~15 turns. Needs validation.

### Visuals
**3. Tile placement visuals**
What does placement actually look and feel like on screen? Animation, sound, neighbor reactions. Drives the Dorf Romantik feel target.

**4. Species card illustration generation method**
How are creature illustrations actually produced? Procedural composition, AI generation, hand-drawn library, hybrid? Major art pipeline decision.

### Meta Progression
**5. Legacy Points currency model**
- Single currency earned every run, OR
- Separate currencies per species path (aquatic runs earn ocean points, etc.), OR
- Milestone based — just reach X evolution level

**6. Unlock order: linear vs. player-chosen**
- Linear → simpler, guided progression
- Player choice → more replayability, different strategies per player

### Shelved (revisit later)
**7. Tile/Threat environmental interaction**
Certain tile configurations could affect threat vulnerability (e.g. ice age punishes low Vitality, mitigated by warm tiles). Shelved until prototype loop is proven.

**8. Compound multi-stat threats**
Threats that punish more than one low stat. Shelved for prototype simplicity.

**9. "Make Fertility more playable mid-run"**
Fertility is currently set early and goes static. Watch in playtest — if it feels passive, add active mid-run interactions (tile-based pool growth, branch events that expand it). Don't pre-solve.

---

## CUT FROM PRIOR VERSIONS

- **Helix visualization** (v0.6) — species card and chronicle cover its jobs more directly. Diagrams aren't shareable; creatures are.
- **Biomass / single resource layer** (v0.5) — collapsed into tile placement directly. The board is the genome.
- **Metabolism stat** (v0.6) — replaced by Adaptability. Operated on a different (resource) layer than Vitality and Fertility, which created an asymmetry. Adaptability is on the population layer alongside the others, with a clean active verb.

---

*Next session: pick up from Open Considerations. Likely starting points are countdown length tuning (last core-loop piece before prototype playtest) or species card illustration method (longest art-pipeline lead time).*
