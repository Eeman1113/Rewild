# Development Roadmap: Rewild

*Created: 2026-04-20*
*Status: Draft*

---

## Vision to Release: Scope Tiers

The roadmap is structured as concentric rings of scope. Each tier is a complete, playable product that tests a specific hypothesis. No tier assumes the next tier will be built -- each one must stand on its own as a meaningful validation of the game's viability.

The core question driving the entire project: **"Is surviving and exploring a living, indifferent post-human world intrinsically compelling?"**

---

## Tier 0: Prototype (Month 1-2)

### Purpose

Prove the riskiest unknowns before committing to the MVP. Prototypes are throwaway code. The goal is answers, not assets.

### What to Prove First

These are ordered by risk -- the thing most likely to kill the project is prototyped first.

**1. Creature AI readability (Week 1-3)**
Can a player watch a creature for 30 seconds and understand what it is doing and why? Build 3 creatures (predator, prey, scavenger) in a single-screen test arena with basic behavioral AI (forage, flee, hunt, rest). No art -- colored rectangles. Test with 3-5 people. If observers cannot narrate what the creatures are doing ("that one is hunting, that one is hiding, that one is scavenging"), the core premise fails.

*Kill criterion: If creature behavior is not readable without explanation after two weeks of iteration, the ecological AI approach needs fundamental redesign before proceeding.*

**2. Movement feel (Week 2-4)**
Does the player controller feel like piloting a body through a space that wasn't built for you? Implement the core movement spec (ground movement, jumping, wall interactions, coyote time, jump buffering) in a test level with varied surfaces. No creatures, no ecology. Pure feel test. Compare against Rain World's slugcat for weight and commitment, against Celeste for responsiveness. The target is between these poles.

*Kill criterion: If the movement does not feel meaningfully different from a generic platformer after tuning, the "trespass" fantasy is not landing through mechanics alone.*

**3. Ecological tension without combat (Week 3-5)**
Can traversing a space occupied by AI creatures generate tension comparable to a designed encounter? Place the prototype creatures in a multi-screen traversal challenge. The player must cross from one side to the other. No weapons, no combat. Only movement, observation, and timing. Does the player feel something when a predator's patrol route intersects theirs? Does relief arrive when they reach the other side?

*Kill criterion: If traversal through creature territory feels like avoiding obstacles rather than surviving in an ecosystem, the game needs designed encounters -- which violates Pillar 1.*

**4. Diegetic information delivery (Week 4-6)**
Can the player read their creature's state without a HUD? Implement sprite degradation for health (progressive visual damage), breathing rate for stamina, and posture for alertness. Test: can players accurately report their creature's health, stamina, and danger level from the sprite alone?

*Kill criterion: If players cannot read their creature's state within 10% accuracy after 15 minutes of play, some HUD elements may be unavoidable. This does not kill the project but reshapes the UI commitment.*

### Prototype Deliverables

| Deliverable | Tests | Pass Condition |
|-------------|-------|----------------|
| Creature behavior arena | AI readability | 3/5 testers narrate creature behavior correctly |
| Movement test level | Controller feel | Movement rated "weighty but responsive" in blind comparison |
| Traversal gauntlet | Ecological tension | Players report tension/relief comparable to designed encounters |
| Diegetic state test | No-HUD viability | Players read creature state within 10% accuracy |

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Creature AI too opaque for observation-based learning | High | Critical | Iterative behavior visualization, exaggerated body language |
| Movement feel lands in uncanny valley (too heavy or too floaty) | Medium | High | Rapid iteration with reference recordings from Rain World / Celeste |
| Ecological tension insufficient without combat | Medium | Critical | Test creature aggression range and patrol density as difficulty levers |
| Diegetic UI unreadable at 480x270 | Medium | Medium | Fallback: minimal ambient indicators (screen-edge color shifts) |

---

## Tier 1: MVP (Month 2-5)

### Core Hypothesis

"Is the core loop -- observe, traverse, survive, evolve -- compelling enough to sustain 60-90 minutes of play?"

### Content Scope

| Category | Count | Details |
|----------|-------|---------|
| Biomes | 3 | Fresh ruin (early succession), mid-decay transitional zone, fully reclaimed deep nature -- showing the full ecological gradient |
| Creatures | 8-10 | 2-3 predator species, 3-4 prey species, 2-3 scavenger/ambient species, each with distinct behavioral profiles |
| Evolution branches | 1 | Single evolution branch with 3-4 mutation stages, enough to feel the creature changing |
| Ruin fragments | 3-5 | One narrative thread about humanity's cognitive decline, told through environmental discovery |
| Shelters | 4-6 | At least 2 per biome, defining the session structure |
| Weather states | 2-3 | Clear, rain, and one biome-specific state (fog, heat shimmer, or spore bloom) |

### Systems Required (15 of 22 total)

**Foundation (3):**
- Input System -- remappable input with analog support
- Scene Management -- biome loading, transitions, adjacent-biome awareness
- Game State Machine -- play, pause, shelter rest, death, respawn

**Core (4):**
- Player Controller -- full movement spec as designed, all states, feel layer with at minimum CALM and TENSE contexts
- Camera System -- follow behavior, room transitions, parallax, the Uninvited Frame composition
- World/Biome System -- 3 biomes with tileset rendering, collision, one-way platforms, surface types
- Time and Weather System -- day/night cycle, 2-3 weather states, creature behavior triggers

**Gameplay (4):**
- Creature AI System -- behavioral reasoning for 8-10 species (forage, flee, hunt, rest, territorial display), readable body language
- Population and Spawning -- ecological distribution rules per biome, population floors and ceilings
- Biological Evolution System -- one branch, 3-4 mutations, triggered by environmental survival, visible on sprite
- Shelter and Rest System -- save points, session anchors, the exhale moment

**Narrative (1):**
- Narrative Discovery System -- ruin interaction, fragment collection, environmental storytelling nodes

**Presentation (2):**
- Diegetic UI System -- health via sprite degradation, stamina via breathing, alertness via posture
- Player Visual System -- base creature sprite, evolution variant sprites, damage states

**Persistence (1):**
- Save/Load System -- Tier 1 persistence (biome summaries, player evolution, world flags)

### "Good Enough" Floors

These define the minimum acceptable quality for each MVP system. Below this line, the system is not ready for testing. Above this line, further polish is deferred to later tiers.

| System | Good Enough Floor | Not Good Enough |
|--------|------------------|-----------------|
| Player Controller | Responsive ground movement, jumping, wall jump, one climb surface type. Feel layer with CALM/TENSE. Coyote time and jump buffering. | Missing variable jump height. No feel layer differentiation. Floaty or unresponsive controls. |
| Creature AI | Creatures have 3+ distinct behavioral states with readable transitions. Predators hunt. Prey flees. Behavior changes with weather or time of day. | Creatures patrol fixed paths. No behavioral response to player or environment. States not visually readable. |
| Biomes | 3 distinct biomes with unique tilesets, surface types, and ecological identity. Transitions between them feel like entering a new ecosystem. | Palette swaps of the same tileset. No environmental storytelling. Biomes feel like levels, not ecosystems. |
| Evolution | Player creature visibly changes after 2+ mutations. Changes affect at least one movement parameter. Evolution feels like the creature adapting, not the player unlocking abilities. | Evolution is a stat boost with no visual change. Mutations feel like a skill tree. No connection to environment survived. |
| Weather | Weather changes creature behavior in observable ways. Rain changes at least one traversal surface. Weather shifts are atmospherically communicated (sky, particles, sound). | Weather is cosmetic only. No gameplay impact. Weather changes are instantaneous with no atmospheric buildup. |
| Narrative | 3-5 ruin fragments are discoverable, each communicating one clear idea about humanity's fall. Discovery feels archaeological, not scripted. | Text dumps. Cutscenes. Narrator voice. Fragments don't connect to anything visible in the world. |
| Diegetic UI | Player can assess creature health, stamina, and danger level from the sprite and environment alone. No HUD elements. | Health bar. Stamina bar. Minimap. Quest markers. Any persistent on-screen UI element. |
| Save/Load | Player can quit and resume from last shelter with evolution and world flags preserved. | Progress lost on quit. Creatures reset to initial positions on load (world feels artificial). |

### MVP Milestones

| Milestone | Target | Deliverable | Depends On |
|-----------|--------|-------------|------------|
| M1: Foundation | Month 2, Week 2 | Input, scene management, game state machine functional. Empty biome loads and transitions. | Prototype results |
| M2: Movement | Month 2, Week 4 | Player controller in first biome with full movement spec. Feel layer operational. | M1 |
| M3: World | Month 3, Week 2 | 3 biomes with tilesets, surface types, transitions. Camera system with parallax. | M1 |
| M4: Creatures | Month 3, Week 4 | 8-10 creature species with behavioral AI in biomes. Population system distributing them ecologically. | M2, M3 |
| M5: Ecology | Month 4, Week 2 | Weather system driving creature behavior changes. Day/night cycle. The world feels alive without the player doing anything. | M3, M4 |
| M6: Evolution | Month 4, Week 3 | One evolution branch with 3-4 mutations. Visual changes on creature. Movement parameter modifications. | M2 |
| M7: Narrative | Month 4, Week 4 | 3-5 ruin fragments placed in biomes. Discovery interaction. One narrative thread readable. | M3 |
| M8: Loop | Month 5, Week 1 | Shelters, save/load, diegetic UI. Complete loop: leave shelter, traverse, survive, discover, evolve, find new shelter. | M4, M5, M6, M7 |
| M9: MVP Playtest | Month 5, Week 2-3 | First external playtest. 5-10 testers play 60-90 minutes. Data collection on core loop engagement. | M8 |

### Critical Path

```
M1 (Foundation) --> M2 (Movement) --> M4 (Creatures) --> M5 (Ecology) --> M8 (Loop)
M1 (Foundation) --> M3 (World) -----> M4 (Creatures)
                                  --> M5 (Ecology)
                                  --> M7 (Narrative)
M2 (Movement) --> M6 (Evolution) --> M8 (Loop)
```

The critical path runs through Foundation, Movement, World, Creatures, Ecology, and Loop. Creature AI is the longest single milestone and the highest-risk system on the critical path. World and Movement can be developed in parallel after Foundation.

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Creature AI takes longer than 2 weeks | High | Delays M4, M5, M8 | Establish behavior complexity ceiling early. Start with 5 creatures, add 3-5 if time allows. |
| 3 biomes insufficient to show ecological gradient | Medium | Core thesis untestable | Ensure biomes are maximally distinct (fresh concrete vs. fungal cavern vs. canopy forest) |
| Evolution branch feels like an ability unlock, not biological adaptation | Medium | Pillar 4 violation | Tie mutations to specific environmental exposure. Visual change must precede mechanical change. |
| Core loop does not sustain 60 minutes | Medium | Project pivot or kill | Identify engagement drop-off point in playtest. Diagnose whether the problem is variety, tension pacing, or reward spacing. |
| Diegetic UI unreadable in practice | Medium | Forced HUD addition | Test with non-gamers. If readability fails, add minimal ambient indicators before adding HUD. |

---

## Tier 2: Vertical Slice (Month 5-8)

### Core Hypothesis

"Can the game sustain 3-4 hours of play with expanding scope, creature migration, and full evolution?"

### Content Scope

| Category | Count | Delta from MVP |
|----------|-------|----------------|
| Biomes | 5 | +2 biomes (e.g., flooded subway network, thermal server farm) |
| Creatures | 15-20 | +7-10 species including first megapredator, first pack hunter, first ambush species |
| Evolution branches | Full tree | All branches unlocked, 8-12 total mutation endpoints |
| Ruin fragments | 12-15 | +9-10, completing one full narrative arc |
| Shelters | 10-12 | +6, including hidden and conditional shelters |
| Weather states | 5-6 | +3 biome-specific states |

### New Systems (5)

| System | What It Adds |
|--------|-------------|
| Environmental Hazard System | Toxic zones, thermal vents, flooded passages, electrical hazards -- traversal obstacles gated by evolution |
| Metroidvania Progression | Ecological gating: areas blocked by creature density, weather, or environmental conditions that require specific evolutions |
| Creature Visual System | Full behavioral animation sets, species-specific visual language, readable at distance |
| World Rendering Pipeline | Weather-affected rendering, biome-specific atmospheric effects, the full Decay Gradient palette system |
| Adaptive Audio System | Ecosystem soundscaping, creature vocalizations, weather audio, tension/calm transitions |

### Key Additions Beyond Content

- **Migration:** Creatures move between adjacent biomes based on weather, season, and population pressure. The world is no longer static between play sessions.
- **Full evolution tree:** All branches available. Evolution path determines which biomes are accessible. Soft-lock prevention system operational.
- **Ecological gating:** Progression is blocked by ecological conditions (a predator density too high to traverse, a weather pattern too harsh to survive, a toxic zone requiring specific adaptation) rather than locked doors.
- **One complete narrative arc:** A full thread of humanity's cognitive decline, from early warning signs to total devolution, told across 12-15 ruin fragments spread across all 5 biomes.
- **Audio as information system:** Creature vocalizations communicate behavior. Weather audio forecasts changes. The soundscape is a survival tool.

### "Good Enough" Floors

| System | Good Enough Floor |
|--------|------------------|
| Migration | Creatures visibly move between biomes. Migration correlates with weather or time. Player can observe and predict migration. |
| Ecological gating | At least 3 progression gates that require observation + evolution to pass. No gates that feel like locked doors. |
| Full evolution tree | 8+ endpoints. No dead-end paths that prevent completion. Evolution graph validator confirms all paths reach all critical biomes. |
| Environmental hazards | 3+ hazard types that interact with evolution and weather. Hazards feel like ecosystem features, not game obstacles. |
| Adaptive audio | Audio state changes are smooth (no hard cuts). Creature sounds are species-distinct. Weather audio precedes visual weather changes. |

### Milestones

| Milestone | Target | Deliverable |
|-----------|--------|-------------|
| VS1: Biome expansion | Month 5, Week 3 | 2 new biomes integrated with transitions and unique tilesets |
| VS2: Creature expansion | Month 6, Week 2 | 15-20 creatures with full behavioral sets, including first megapredator |
| VS3: Migration and ecology | Month 6, Week 4 | Creatures migrate between biomes. Ecological gating operational. |
| VS4: Full evolution | Month 7, Week 2 | Complete evolution tree. Soft-lock validator passing. |
| VS5: Presentation layer | Month 7, Week 4 | Full creature visuals, world rendering pipeline, adaptive audio |
| VS6: Narrative arc | Month 8, Week 1 | 12-15 ruin fragments, one complete narrative arc |
| VS7: VS Playtest | Month 8, Week 2-3 | Extended playtest: 10-15 testers, 3-4 hour sessions |

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Migration creates degenerate ecosystem states | High | Creatures bunch up or go extinct in biomes | Population floors/ceilings. Migration dampening. Automated ecosystem health checks. |
| Evolution soft-locks despite validator | Medium | Player cannot progress | Graph validator runs on every build. Manual path testing for each endpoint combination. |
| 5 biomes strain art production capacity | High | Art bottleneck delays everything | Establish tileset pipeline with modular components. Biome 4 and 5 reuse structural elements from 1-3 with new biological overlays. |
| Ecological gating feels arbitrary | Medium | Pillar 1 violation | Every gate must have an observable ecological reason. Playtest specifically for "did this feel like a locked door?" |
| Audio system scope creep | Medium | Delays VS5 | Define fixed sound budget per biome. Prioritize creature vocalizations over ambient variety. |

---

## Tier 3: Alpha (Month 8-16)

### Core Hypothesis

"Does the full vision work at scale -- 10 biomes, 30+ creatures, seasonal cycles, ecosystem persistence, complete narrative?"

### Content Scope

| Category | Count | Delta from VS |
|----------|-------|---------------|
| Biomes | 10 | +5 biomes spanning full succession gradient and environmental extremes |
| Creatures | 30+ | +15 species including specialized ecological roles (symbiotes, parasites, apex migrants) |
| Evolution branches | Full tree, polished | All paths balanced, visual progression refined |
| Ruin fragments | 25-30 | +13-15, completing 3 full narrative arcs |
| Shelters | 20+ | Including evolved shelters, temporary shelters, and contested shelters |
| Weather states | Full system | All weather types, seasonal variation, biome-specific extremes |

### New Systems (2)

| System | What It Adds |
|--------|-------------|
| Decay and Succession System | Long-term environmental change. Ruins degrade further over play time. Ecological succession progresses. The world visibly ages. |
| Settings and Accessibility | Remappable controls, colorblind modes, difficulty modifiers (optional creature aggression tuning, not a traditional difficulty slider) |

### Key Additions

- **Seasonal cycles:** Weather follows seasonal patterns. Creature behavior shifts with seasons. Some biomes are only accessible during certain seasons.
- **Ecosystem persistence:** Creature populations, territorial boundaries, and resource distribution persist between sessions. The world remembers what happened.
- **Decay and succession:** Ruins degrade over real play time. New growth appears. The world ages alongside the player.
- **Complete narrative:** 3 full narrative arcs spanning 25-30 ruin fragments. The complete story of humanity's cognitive decline and devolution is discoverable.
- **Accessibility:** The game's difficulty philosophy is preserved, but accessibility options ensure the difficulty is about comprehension and mastery, not about input barriers.

### "Good Enough" Floors

| System | Good Enough Floor |
|--------|------------------|
| Seasonal cycles | 4 seasons with distinct weather and creature behavior profiles. Season transitions are atmospheric, not instant. |
| Ecosystem persistence | Population changes from player actions persist across 3+ play sessions. Consequences are observable. |
| Decay and succession | At least 3 visible stages of ruin degradation. Players who revisit areas after 5+ hours notice change. |
| Full narrative | All 3 arcs completable. No fragments that require specific evolution paths to reach (narrative is accessible to all builds). |

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| 10 biomes impossible for solo art production | High | Content bottleneck | Contract pixel artist for tileset production. Maintain creative direction control. Modular tileset system. |
| Ecosystem persistence creates save file bloat | Medium | Technical debt | Tiered save system. Tier 2 data (creature states) compressed. Tier 3 data (decorative) regenerated from seed. |
| Seasonal cycles make some content inaccessible for too long | Medium | Player frustration | Seasons shorter than real-time. Fast-forward at shelters. No content that requires waiting more than one season. |
| Narrative arc quality uneven across 25-30 fragments | Medium | Story impact diluted | Each arc reviewed as a standalone sequence. Cut fragments that don't earn their discovery. |
| Scope creep from Alpha ambition | High | Timeline overrun | Feature-locked milestone at Alpha entry. No new systems. Content and polish only. |

---

## Tier 4: Beta / Release (Month 16-24)

### Core Hypothesis

"Is the game worth buying? Is it complete, polished, and emotionally resonant from start to finish?"

### Content Scope

| Category | Count | Delta from Alpha |
|----------|-------|-----------------|
| Biomes | 16+ | +6 biomes completing the full world map |
| Creatures | 50+ | +20 species filling all ecological niches |
| Evolution branches | Full tree, balanced | All paths playtested and balanced for full game |
| Ruin fragments | 40+ | +15, completing the full narrative with hidden deep-lore fragments |
| Shelters | 30+ | Full shelter network with multiple shelter types |
| Weather states | Complete | All weather, all seasons, all biome-specific extremes |

### Focus Areas

Beta is not about new systems. Every system is feature-complete at Alpha. Beta is about:

**1. Content completion**
- Remaining 6+ biomes with full tilesets, creatures, and ecological identities
- Remaining 20+ creature species with complete behavioral profiles and animations
- Final 15+ ruin fragments including hidden deep-lore discoveries
- Full world map connectivity and traversal balancing

**2. Balance and tuning**
- Creature behavior tuning across all biomes and seasons
- Evolution path balancing -- every endpoint viable for full completion
- Difficulty curve from first biome to final biome
- Weather system tuning for gameplay impact vs. frustration
- Session pacing -- shelter spacing, danger density, rest-to-danger ratios

**3. Polish**
- Animation polish across all creature species and evolution variants
- Particle effects, atmospheric rendering, environmental detail
- Audio polish -- mixing, spatial audio, transition smoothing
- Save system robustness -- corruption prevention, backup saves
- Performance optimization -- simulation LOD for 16+ biomes, frame budget compliance

**4. Quality assurance**
- Full regression testing across all biomes, creatures, and evolution paths
- Soft-lock testing -- automated and manual verification that all paths reach completion
- Platform testing -- Mac and PC, variety of hardware configurations
- Accessibility testing -- colorblind modes, remapping, assistive features
- Playtest program -- 30+ testers, full playthroughs, data collection

### Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Art production for 16+ biomes and 50+ creatures | High | Ship delay | Prioritize biomes by narrative importance. Ship with 12 biomes if needed (the remaining 4 become post-launch content). |
| Performance at full world scale | Medium | Technical crisis | Simulation LOD must be validated by month 18. Profile monthly. |
| Balance across 50+ creatures and all evolution paths | High | Degenerate strategies or soft-locks | Automated testing suite. Community beta testing. Evolution graph validator runs continuously. |
| Player retention over 40+ hours | Medium | Game feels padded | Playtest for engagement drop-off. Cut content that feels like filler. 30 compelling hours beats 40 padded hours. |

---

## Dependencies Between Milestones

```
PROTOTYPE
  Proto-1 (Creature AI Readability) ----> M4 (Creatures)
  Proto-2 (Movement Feel) --------------> M2 (Movement)
  Proto-3 (Ecological Tension) ---------> M4, M5
  Proto-4 (Diegetic UI) ----------------> M8 (Loop)

MVP
  M1 (Foundation) -------> M2 (Movement) -------> M4 (Creatures) -------> M8 (Loop)
                    |                       |
                    +-----> M3 (World) -----+-----> M5 (Ecology) --------> M8
                                      |
                                      +-----> M7 (Narrative) -----------> M8
  M2 (Movement) ---------> M6 (Evolution) -------> M8 (Loop)

VERTICAL SLICE
  M8 (Loop) -----> VS1 (Biomes) -----> VS3 (Migration) -----> VS7 (Playtest)
             |                    |
             +---> VS2 (Creatures) ----> VS3
             |
             +---> VS4 (Evolution) ---> VS7
             |
             +---> VS5 (Presentation) -> VS7
             |
             +---> VS6 (Narrative) ----> VS7

ALPHA
  VS7 (VS Playtest) -----> A1 (Biome Expansion) -----> A3 (Seasons + Persistence)
                     |
                     +----> A2 (Creature Expansion) --> A3
                     |
                     +----> A4 (Narrative Completion)
                     |
                     +----> A5 (Decay System)
                     |
                     +----> A6 (Accessibility)

BETA
  Alpha Complete -----> B1 (Content Completion) -----> B3 (Polish)
                  |                              |
                  +---> B2 (Balance + Tuning) ----> B3
                                                 |
                                                 +---> B4 (QA) -----> RELEASE
```

---

## Critical Path Analysis

The critical path through the entire project:

**Prototype** (Creature AI readability) --> **MVP** (Foundation --> Movement + World --> Creatures --> Ecology --> Loop) --> **Vertical Slice** (Biome expansion + Creature expansion --> Migration/Ecology --> Playtest) --> **Alpha** (Content scale-up --> Seasonal systems) --> **Beta** (Balance --> Polish --> QA) --> **Release**

### Bottleneck Systems

**1. Creature AI (highest risk, longest development)**
Every tier depends on creature AI being good enough. At MVP, 8-10 creatures with readable behavior. At VS, 15-20 with migration. At Alpha, 30+ with seasonal variation. At Beta, 50+ fully polished. This system is the single greatest risk to the project and must be prototyped first, validated at MVP, and iteratively expanded. If creature AI is not compelling at MVP, nothing else matters.

**2. Art Production (highest volume, most labor-intensive)**
Every biome needs unique tilesets. Every creature needs behavioral animation sets. Every evolution variant needs sprite modifications. Art production scales linearly with content and cannot be parallelized without additional artists. By Alpha, a contract artist is likely necessary.

**3. Evolution System (highest design complexity)**
The evolution tree must allow all players to reach all critical content regardless of their path. This requires automated validation and grows exponentially harder with each new branch. The graph validator is not optional -- it is infrastructure.

---

## Quality Gates

Each tier must pass its quality gate before the next tier begins. Gates are binary -- pass or fail. A failed gate means the current tier is iterated on, not that work begins on the next tier with known deficits.

### Gate 0: Prototype --> MVP

| Criterion | Pass Condition |
|-----------|---------------|
| Creature readability | 3/5 testers narrate creature behavior correctly without instruction |
| Movement feel | Movement rated "weighty but responsive" by 3/5 testers |
| Ecological tension | Traversal through creature territory generates measurable tension (elevated engagement, relief on completion) |
| Diegetic UI | Players read creature state within 10% accuracy after 15 minutes |
| Decision | Core hypothesis is worth testing at MVP scale |

### Gate 1: MVP --> Vertical Slice

| Criterion | Pass Condition |
|-----------|---------------|
| Core loop engagement | 4/5 testers play 60+ minutes without disengaging. Median session length exceeds 45 minutes. |
| Creature AI | Testers can explain at least 3 creature behaviors they observed and understood |
| Ecological indifference | Testers report the world "feels alive" and "doesn't revolve around them" |
| Evolution resonance | Testers notice and comment on their creature's evolution without being prompted |
| Pillar validation | Each pillar is validated by at least one specific playtest observation |
| Technical stability | No crashes in 80% of test sessions. Save/load works correctly. Frame rate stable at 60fps. |

### Gate 2: Vertical Slice --> Alpha

| Criterion | Pass Condition |
|-----------|---------------|
| Sustained engagement | 4/5 testers play 3+ hours across multiple sessions. Testers voluntarily return for session 2. |
| Migration readability | Testers observe and comment on creature migration without prompting |
| Ecological gating | Testers understand why they cannot access certain areas (ecological reason, not "it's locked") |
| Evolution tree viability | Graph validator confirms all paths reach all critical biomes. No reported soft-locks in testing. |
| Narrative resonance | Testers piece together at least one narrative thread and express curiosity about the rest |
| Audio contribution | Testers report using sound to make gameplay decisions (hearing predators, weather changes) |
| Performance | 60fps stable across all 5 biomes with 15-20 creatures active. Load times under 2 seconds. |

### Gate 3: Alpha --> Beta

| Criterion | Pass Condition |
|-----------|---------------|
| Scale validation | 10 biomes function without ecosystem degeneration over 10+ hours |
| Content depth | Testers report 8+ hours of unique, non-repetitive content |
| Narrative completeness | All 3 narrative arcs are discoverable and comprehensible. Testers reconstruct the story of humanity's fall. |
| Seasonal impact | Testers observe and adapt to seasonal changes. Seasons feel like ecosystem events, not timers. |
| Ecosystem persistence | Testers notice and respond to world-state changes between sessions |
| Accessibility | All accessibility features functional. Game completable with accessibility options enabled. |
| Performance at scale | 60fps stable with 10 biomes, 30+ creatures, seasonal weather, ecosystem persistence |

### Gate 4: Beta --> Release

| Criterion | Pass Condition |
|-----------|---------------|
| Completion rate | 30%+ of testers reach the final biome in a full playthrough |
| Session retention | Testers average 3+ sessions voluntarily. 20%+ complete the game. |
| Emotional resonance | Post-playtest surveys indicate emotional engagement with the world and narrative |
| Technical quality | Zero crash bugs. Zero soft-locks. Save corruption rate below 0.1%. |
| Performance | 60fps on minimum spec hardware. Load times under 3 seconds. |
| Platform validation | Mac and PC builds tested on 5+ hardware configurations each |
| Store readiness | Steam page, trailer, screenshots, description finalized |

---

## Timeline Summary

| Phase | Months | Calendar Target | Key Deliverable |
|-------|--------|-----------------|-----------------|
| Prototype | 1-2 | Month 1-2 | Risk validation: creature AI, movement, tension, diegetic UI |
| MVP | 2-5 | Month 2-5 | Core loop playable: 3 biomes, 8-10 creatures, first evolution, first ruins |
| Vertical Slice | 5-8 | Month 5-8 | Extended experience: 5 biomes, 15-20 creatures, migration, full evolution, one narrative arc |
| Alpha | 8-16 | Month 8-16 | Feature-complete: 10 biomes, 30+ creatures, seasons, persistence, full narrative |
| Beta | 16-24 | Month 16-24 | Content-complete: 16+ biomes, 50+ creatures, polished and balanced |
| Release | 24 | Month 24 | Ship on Steam (Mac/PC) |

### Schedule Contingency

If the timeline compresses or extends:

**If ahead of schedule:** Add biome variety and creature count. Do not add new systems. More content within proven systems is always safer than new mechanics.

**If behind schedule by 1-3 months:** Cut biome count at Beta (ship with 12 biomes instead of 16). Reduce creature count to 40. These are content reductions that do not compromise the core experience.

**If behind schedule by 3-6 months:** Ship at Alpha scope (10 biomes, 30+ creatures) as a smaller but complete game. Market as a focused ecological Metroidvania rather than an epic. This is viable -- 10 biomes with 30+ creatures is still a substantial game.

**If behind schedule by 6+ months:** Evaluate whether the project is viable at current pace. Consider Early Access at Vertical Slice quality (5 biomes, 15-20 creatures) to fund continued development.

---

## Prototype Priorities (What to Prove First)

Ordered by risk to the project's viability:

1. **Creature AI readability** -- If players cannot read creature behavior, the ecological premise fails. Test first.
2. **Movement feel** -- If the controller does not feel like piloting a body, the trespass fantasy fails. Test second.
3. **Ecological tension without combat** -- If creature territory traversal is not tense, the game needs designed encounters. Test third.
4. **Diegetic information delivery** -- If players cannot read their creature's state, some HUD is needed. Test fourth.
5. **Evolution visual feedback** -- If players do not notice their creature changing, Pillar 4 is broken. Can be tested at MVP.
6. **Ruin narrative delivery** -- If environmental storytelling does not land, narrative structure needs revision. Can be tested at MVP.
7. **Weather-creature interaction** -- If weather does not visibly affect creature behavior, the ecology feels scripted. Can be tested at MVP.

Items 1-4 are prototype-phase. Items 5-7 can wait for MVP because they build on systems proven by items 1-4.
