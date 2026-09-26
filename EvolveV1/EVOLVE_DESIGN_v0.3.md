# EVOLVE — High Level Design Document
**Version:** 0.3  
**Status:** In Progress  
**Last Updated:** March 2026

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

- Population meter hits zero AND no resources remain to spend
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

## CORE LOOP — SINGLE TURN

```
1. PLAYER PHASE  → place an environment tile
2. GENERATE      → tiles produce resources based on neighbors
3. WORLD SHIFT   → threat TYPE is revealed to player
4. EVOLVE        → player spends resources to respond
5. RESOLVE       → threat MAGNITUDE is rolled randomly, population adjusts
```

**Key design note:** Player sees *what* threat is coming but not *how hard* it hits.  
This creates four satisfying outcomes:
- Prepared + lucky → easy round
- Prepared + unlucky → survived but hurt
- Unprepared + lucky → got away with it
- Unprepared + unlucky → genuine danger

---

## RESOURCE SYSTEM

- **Single resource:** Biomass
- **Base generation:** 1 Biomass per tile per turn
- **Neighbor bonus:** +1 Biomass per matching adjacent tile
- **Species binding:** Species type determines which tile types generate Biomass

**Example:**
```
3 Ocean tiles in a row     = 1+2+2 = 5 Biomass
3 isolated Ocean tiles     = 1+1+1 = 3 Biomass
```
Clustering is rewarded. Misfit tiles still contribute base Biomass, never punished.

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

Three starting stats. Each maps directly to one threat category.

| Stat | Concept | Effect | Tradeoff |
|---|---|---|---|
| **Vitality** | Physical resilience, hardiness | Reduces population damage taken on RESOLVE | Keeps you alive but doesn't help you grow |
| **Fertility** | Reproductive rate | Recovers population between turns | Great recovery but doesn't protect against big hits |
| **Metabolism** | Efficiency, living on less | Reduces Biomass cost during scarce turns | Softens resource pressure but doesn't directly protect population |

**The tradeoff triangle:**
```
Vitality   → don't get hurt
Fertility  → recover when you do
Metabolism → need less to survive
```
No single stat does everything. Tension between all three drives decision-making in the EVOLVE step.

---

## THREAT SYSTEM

### Three Categories (Prototype)
Each threat targets one stat. Players see the category before resolving.

| Category | Punishes | Feel |
|---|---|---|
| **Environmental** | Low Metabolism | Food/resource scarcity |
| **Predatory** | Low Vitality | Physical danger |
| **Epidemic** | Low Fertility | Population collapse |

### Threat Anatomy
```
Type      → which category (revealed on WORLD SHIFT step)
Severity  → how hard it hits (hidden until RESOLVE roll)
```

### Threat Severity Scaling
- Scales with run length, not on a fixed cycle
- Early game: forgiving rolls
- Late game: punishing rolls, existential stakes

### Example Threats by Category

**Environmental (punish low Metabolism)**
```
Early:  Drought, Volcanic Winter, Ocean Acidification
Mid:    Toxic Bloom, Resource Collapse
Late:   Gravity Shift, Dimensional Leak
```

**Predatory (punish low Vitality)**
```
Early:  Apex Predator Emerges, Parasite Outbreak, Territory Invasion
Mid:    Pack Hunters, Venomous Competitor
Late:   Psychic Predator, Crystalline Plague
```

**Epidemic (punish low Fertility)**
```
Early:  Disease Outbreak, Genetic Bottleneck, Nesting Disruption
Mid:    Mutagenic Spore, Sterility Pathogen
Late:   Memory Virus, Entropy Cascade
```

*Late/weird threats only appear after sufficient meta progression unlocks. New players never see a Psychic Predator. Weirdness is earned.*

---

## SPECIES IDENTITY SYSTEM

Species identity emerges automatically from player choices — no direct input required. The surprise and discovery IS the reward.

### Identity Sources

**Source 1: Stat Profile**
```
High Vitality   → heavily armored, robust creatures
High Fertility  → swarming, colonial, numerous creatures
High Metabolism → lean, strange, efficient creatures
```

**Source 2: Tile Path**
```
Water heavy → aquatic, fluid, tentacled forms
Land heavy  → grounded, limbed, territorial forms
Mixed       → amphibious, transitional, weird hybrids
```

Species identity = intersection of stat profile + tile path.  
*Example: High Vitality + Water = armored shell creatures*

### Branch Events

Two distinct event types fire at different rates during a run:

**Branch Event Type 1: Trait Moment (flavor)**
- Purely cosmetic, fires more frequently
- Player chooses from 2-3 visual/descriptive options
- No mechanical impact, shapes species identity and story
```
Example:
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
Example:
"Your species is developing neural complexity.
→ Neural Cluster unlocked (Intelligence stat added)"
```

---

## META PROGRESSION

Connects runs together. Funds Pillar 4 — each run makes the next run richer.

### Pillar 1: Stat Unlocks
Unlocks more advanced stats available at later evolution levels in future runs.

```
Always available:  Vitality, Fertility, Metabolism
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

---

## OPEN CONSIDERATIONS (to revisit)

**1. Legacy Points Currency**
What funds meta progression unlocks?
- Single currency earned every run
- Separate currencies per species path (aquatic runs earn ocean points, etc.)
- Milestone based — just reach X evolution level

**2. Unlock Order**
Are meta progression unlocks linear or player-chosen?
- Linear → simpler, guided progression
- Player choice → more replayability, different strategies per player

**3. Branch Event Ratio**
How frequently do trait moments fire vs stat unlocks?
Needs a rough ratio before implementation.
- Rough starting point: trait moment every ~5 turns, stat unlock every ~15 turns?

**4. Metabolism Mechanic Detail**
Does Metabolism reduce Biomass costs generally, or only reduce population loss during resource-scarce turns specifically?
Distinction matters for detailed system design.

**5. Tile/Threat Environmental Interaction**
Parked idea: certain tile configurations could affect threat vulnerability (e.g. ice age punishes low Vitality, mitigated by warm tiles). Shelved for now, can revisit after prototype loop is proven.

---

*Next session: pick up from Open Considerations, then move toward prototype scope definition.*
