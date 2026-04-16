# Systems Index: Rewild

> **Status**: Draft
> **Created**: 2026-04-16
> **Last Updated**: 2026-04-16
> **Source Concept**: design/gdd/game-concept.md

---

## Overview

Rewild is an ecological Metroidvania requiring 22 systems across nine domains: creature simulation (the AI-driven ecosystem at the heart of the game), player mechanics (precision platforming with biological evolution), world systems (biomes, weather, decay), narrative (archaeological storytelling through ruins), presentation (diegetic UI, adaptive audio, visual systems), and persistence (tiered save architecture). The core loop — observe, traverse, survive, evolve — demands that creature AI, player movement, and world systems are tightly coupled but independently designable. Game pillars ("The World Doesn't Care About You," "Mastery Through Understanding," "Every Ruin Tells a Truth," "You Become What You Survive") constrain every system: nothing exists for the player's convenience, everything follows ecological or physical law.

---

## Systems Enumeration

| # | System Name | Category | Priority | Status | Design Doc | Depends On |
|---|-------------|----------|----------|--------|------------|------------|
| 1 | Input System (inferred) | Foundation | MVP | Not Started | — | — |
| 2 | Scene Management (inferred) | Foundation | MVP | Not Started | — | — |
| 3 | Game State Machine (inferred) | Foundation | MVP | Not Started | — | — |
| 4 | Player Controller | Core | MVP | Not Started | — | Input System |
| 5 | Camera System (inferred) | Core | MVP | Not Started | — | Player Controller, World/Biome |
| 6 | World/Biome System | Core | MVP | Not Started | — | Scene Management |
| 7 | Time & Weather System | Core | MVP | Not Started | — | World/Biome |
| 8 | Creature AI System | Gameplay | MVP | Not Started | — | World/Biome, Time & Weather |
| 9 | Population & Spawning (inferred) | Gameplay | MVP | Not Started | — | World/Biome, Creature AI |
| 10 | Biological Evolution System | Gameplay | MVP | Not Started | — | Player Controller, World/Biome |
| 11 | Shelter & Rest System | Gameplay | MVP | Not Started | — | Player Controller, World/Biome |
| 12 | Environmental Hazard System (inferred) | Gameplay | Vertical Slice | Not Started | — | World/Biome, Time & Weather, Player Controller |
| 13 | Metroidvania Progression (inferred) | Gameplay | Vertical Slice | Not Started | — | World/Biome, Biological Evolution, Creature AI |
| 14 | Decay & Succession System | Gameplay | Alpha | Not Started | — | World/Biome, Time & Weather, Narrative Discovery |
| 15 | Narrative Discovery System | Narrative | MVP | Not Started | — | World/Biome, Player Controller |
| 16 | Diegetic UI System (inferred) | UI | MVP | Not Started | — | Player Controller, Biological Evolution |
| 17 | Player Visual System (inferred) | Presentation | MVP | Not Started | — | Player Controller, Biological Evolution |
| 18 | Creature Visual System (inferred) | Presentation | Vertical Slice | Not Started | — | Creature AI |
| 19 | World Rendering Pipeline (inferred) | Presentation | Vertical Slice | Not Started | — | World/Biome, Time & Weather |
| 20 | Adaptive Audio System (inferred) | Audio | Vertical Slice | Not Started | — | World/Biome, Creature AI, Time & Weather |
| 21 | Save/Load System (inferred) | Persistence | MVP | Not Started | — | Game State Machine, World/Biome, Biological Evolution |
| 22 | Settings & Accessibility (inferred) | Meta | Alpha | Not Started | — | Input System, Save/Load |

**Explicit systems** (from game concept Core Mechanics): #4, #7, #8, #10, #14, #15
**Explicit systems** (from Core Loop / Technical Considerations): #6, #11, #21
**Inferred systems**: all others — derived from what the explicit systems require to function

---

## Categories

| Category | Description | Systems in Rewild |
|----------|-------------|-------------------|
| **Foundation** | Infrastructure everything depends on — no game design, pure architecture | Input, Scene Management, Game State Machine |
| **Core** | Systems that define how the player exists in the world | Player Controller, Camera, World/Biome, Time & Weather |
| **Gameplay** | The systems that make the game feel alive and challenging | Creature AI, Population & Spawning, Biological Evolution, Shelter & Rest, Environmental Hazards, Metroidvania Progression, Decay & Succession |
| **Narrative** | Story and discovery delivery | Narrative Discovery |
| **UI** | Player-facing information — fully diegetic in Rewild | Diegetic UI |
| **Presentation** | Visual representation of gameplay systems | Player Visual, Creature Visual, World Rendering Pipeline |
| **Audio** | Sound and atmosphere | Adaptive Audio |
| **Persistence** | Save state and continuity | Save/Load |
| **Meta** | Systems outside the core game loop | Settings & Accessibility |

Categories NOT used in Rewild: Economy (no currency/shops), Progression (evolution IS the progression — it lives in Gameplay).

---

## Priority Tiers

| Tier | Definition | Target Milestone | Systems | Count |
|------|------------|------------------|---------|-------|
| **MVP** | Required for the core loop: observe, traverse, survive, evolve. Tests "is surviving an indifferent ecosystem compelling?" | First playable (3-4 months) | #1-11, #15-17, #21 | 15 |
| **Vertical Slice** | Complete experience in 5 biomes. Full traversal challenge, readable creature behaviors, ecological gating, atmospheric presentation | Vertical slice (6-8 months) | #12, #13, #18-20 | 5 |
| **Alpha** | All features present: long-term decay, full settings, accessibility. Content-complete for 10 biomes | Alpha (12-16 months) | #14, #22 | 2 |
| **Full Vision** | Polish, edge cases, 16+ biomes content-complete | Beta / Release (18-24 months) | — | 0 |

### Priority Rationale

**Why each MVP system is MVP:**

| System | Why MVP | Pillar Served |
|--------|---------|---------------|
| Player Controller | Core loop IS movement — without responsive platforming, the 30-second loop is dead | Pillar 2: Mastery Through Understanding |
| World/Biome System | 3 biomes showing the succession gradient — without the world canvas, no system has context | Pillar 1: The World Doesn't Care |
| Creature AI | 8-10 creatures with behavioral reasoning validate the core hypothesis: is an indifferent ecosystem compelling? | Pillar 1 + Pillar 2 |
| Population & Spawning | Creatures need ecological distribution — random placement violates ecological authenticity | Pillar 1 |
| Time & Weather | Weather drives creature behavior changes — validates that environmental dynamics create emergent gameplay | Pillar 1 |
| Biological Evolution | One evolution branch tests "You Become What You Survive" — player needs to feel their creature changing | Pillar 4 |
| Shelter & Rest | Session anchors — without shelters, the shelter-to-shelter journey loop doesn't exist | Core Loop structure |
| Narrative Discovery | 3-5 ruin fragments test "Every Ruin Tells a Truth" — validates archaeological storytelling | Pillar 3 |
| Diegetic UI | No HUD is a core commitment — health via sprite degradation and environmental wayfinding must work from day one | Anti-pillar: NOT hand-holdy |
| Player Visual | Evolution must be visible on the creature — without visual feedback, Pillar 4 feels abstract | Pillar 4 |
| Save/Load | Progress persistence across sessions — the journey is continuous, not cyclical | Anti-pillar: NOT a roguelike |
| Foundation (×3) | Input, Scene Management, Game State Machine — infrastructure for everything above | All pillars |
| Camera | Player must see the world — platformer requires functional camera | All pillars |

**Why Vertical Slice systems are deferred past MVP:**

| System | Why Not MVP | What Changes at VS |
|--------|-------------|-------------------|
| Environmental Hazards | MVP weather affects traversal; dedicated hazard types (toxic, thermal, etc.) need more biomes to be meaningful | 5 biomes create enough variety for distinct hazard identities |
| Metroidvania Progression | MVP tests the core loop without gates — ecological gating is the biggest open design question and needs prototyping | VS introduces the actual gate structure once core movement + evolution are proven |
| Creature Visual | MVP creatures need basic animation; full behavioral animation suites per species are VS polish | 15-20 creatures demand readable behavioral animations |
| World Rendering | MVP uses basic tilemap + minimal parallax; full 5-layer pipeline with decay overlays comes at VS | VS is where "Quiet Takeover" visual identity must fully land |
| Adaptive Audio | MVP uses placeholder audio; the ecosystem soundscape is a major production effort | VS is where atmosphere needs to carry the experience |

---

## Dependency Map

### Foundation Layer (no dependencies)

1. **Input System** — Maps keyboard/mouse and gamepad input to game actions. All player interaction flows through here.
2. **Scene Management** — Handles biome loading, unloading, and transitions. Foundation for the streaming world.
3. **Game State Machine** — Manages top-level game states (playing, sheltering, evolving, paused, loading). Persistence hooks attach here.

### Core Layer (depends on Foundation)

4. **Player Controller** — depends on: Input System. Precision platforming with context-sensitive movement feel. The beating heart of the 30-second loop.
5. **Camera System** — depends on: Player Controller, World/Biome. Follows player, manages screen transitions, drives parallax viewport.
6. **World/Biome System** — depends on: Scene Management. Tilemap management, biome definitions (3 at MVP → 16+ at full vision), zone boundaries, environmental rule sets per biome.
7. **Time & Weather System** — depends on: World/Biome. Day/night cycle, weather state machine, per-biome weather probability tables, creature behavior modifiers.

### Gameplay Layer (depends on Core)

8. **Creature AI System** — depends on: World/Biome, Time & Weather. Individual creature behavioral state machines: forage, flee, hunt, rest, territorial display. Sensory model (sight, hearing, smell). Food web role definitions. Behavioral reasoning ("why is this creature doing what it's doing?"). The most complex and highest-risk system.
9. **Population & Spawning** — depends on: World/Biome, Creature AI. Per-biome species rosters, population floors and ceilings, spawn distribution rules, simulation LOD (full AI for current + adjacent biomes, statistical models for distant biomes). Migration routes at VS+.
10. **Biological Evolution System** — depends on: Player Controller, World/Biome. Permanent path-dependent mutations triggered by environmental survival. Evolution tree (one branch at MVP, full tree at VS+). Body modification mechanics. Soft-lock prevention graph validator.
11. **Shelter & Rest System** — depends on: Player Controller, World/Biome. Safe zones with ecological justification (no arbitrary save rooms). Rest mechanics, weather protection, session boundary anchors.
12. **Environmental Hazard System** — depends on: World/Biome, Time & Weather, Player Controller. Biome-specific dangers (toxic zones, thermal vents, flooded passages), weather-based threats, traversal challenges distinct from creature threats. *Vertical Slice.*
13. **Metroidvania Progression** — depends on: World/Biome, Biological Evolution, Creature AI. Ecological gates replacing key-lock design: environmental barriers (toxicity requires evolved resistance), creature territory barriers (megapredator blocks passage until migration pattern shifts), weather barriers (storm blocks path until weather cycle changes). The game's biggest open design question. *Vertical Slice.*
14. **Decay & Succession System** — depends on: World/Biome, Time & Weather, Narrative Discovery. Long-term ecological succession: ruins aging through decay states, vegetation overtaking structures, biome visual aging over player progression. Connects the visual timeline to the narrative. *Alpha.*

### Narrative Layer (depends on Core + Gameplay context)

15. **Narrative Discovery System** — depends on: World/Biome, Player Controller. Ruin exploration nodes placed in biomes. Story fragment types (data terminals, environmental scenes, artifact clusters). Discovery tracking. Archaeological storytelling structure (fragments → threads → revelations). No cutscenes, no narrator — all environmental.

### Presentation Layer (depends on Gameplay systems they visualize)

16. **Diegetic UI System** — depends on: Player Controller, Biological Evolution. Health communicated via sprite degradation (4 tiers from art bible). Dorsal ridge compass (4×4 pixel shelter direction — single non-diegetic element). Reflection pool as character sheet. Environmental wayfinding cues. Pause as world stillness. No HUD elements.
17. **Player Visual System** — depends on: Player Controller, Biological Evolution. Context-sensitive animations (tense/precise/calm per game state). Evolution visual variants (dorsal ridge changes, limb modifications, coloration shifts). 16×16 base sprite, 32×32 max with mutations.
18. **Creature Visual System** — depends on: Creature AI. Behavioral animation suites per species (idle, alert, flee, hunt, feed, rest, territorial display). Visual archetype fidelity from art bible (6 categories). Ecological role readability at thumbnail distance. *Vertical Slice.*
19. **World Rendering Pipeline** — depends on: World/Biome, Time & Weather. 5-layer parallax per biome. Decay overlay system (3 architectural eras from art bible). Bioluminescent lighting. Weather particle systems. Day/night palette shifting. *Vertical Slice.*

### Audio Layer (depends on Gameplay systems they sonify)

20. **Adaptive Audio System** — depends on: World/Biome, Creature AI, Time & Weather. Ambient ecosystem soundscape (replaces traditional music). Creature vocalizations that communicate behavioral state. Weather audio. Spatial sound. Silence as design tool. *Vertical Slice.*

### Persistence Layer (cross-cutting)

21. **Save/Load System** — depends on: Game State Machine, World/Biome, Biological Evolution. Three-tier architecture: Tier 1 (always saved: biome summaries, player evolution, world flags), Tier 2 (saved on exit: individual creature states), Tier 3 (regenerated from seed: decorative elements). Shelter as save trigger.

### Meta Layer (depends on Persistence)

22. **Settings & Accessibility** — depends on: Input System, Save/Load. Graphics and audio settings. Input remapping (keyboard + gamepad). Colorblind safety modes (art bible mandates shape/icon/sound backup for semantic colors). *Alpha.*

### Bottleneck Systems (high dependency count)

These systems have many dependents — they are high-risk foundations. Delays or design problems here cascade through the entire project:

| System | Direct Dependents | Risk Level |
|--------|-------------------|------------|
| **World/Biome System** | 15 systems depend on biome data | Critical bottleneck |
| **Player Controller** | 7 systems depend on player state/position | High |
| **Time & Weather** | 6 systems depend on weather state | High |
| **Creature AI** | 4 systems depend on creature behavior | High (also highest complexity) |
| **Biological Evolution** | 4 systems depend on evolution state | Medium |

---

## Recommended Design Order

GDD-worthy systems are ordered by dependency (design referenced systems first) combined with priority tier (MVP before VS before Alpha). Foundation systems (#1-3, #5) receive architecture specs from `/create-architecture` rather than full 8-section GDDs.

| Order | System | Priority | Layer | Primary Agent(s) | Est. Effort |
|-------|--------|----------|-------|-------------------|-------------|
| 1 | Player Controller | MVP | Core | game-designer, gameplay-programmer | L |
| 2 | World/Biome System | MVP | Core | game-designer, systems-designer | L |
| 3 | Creature AI System | MVP | Gameplay | game-designer, ai-programmer | L |
| 4 | Time & Weather System | MVP | Core | systems-designer | M |
| 5 | Population & Spawning | MVP | Gameplay | systems-designer, ai-programmer | M |
| 6 | Biological Evolution System | MVP | Gameplay | game-designer | L |
| 7 | Shelter & Rest System | MVP | Gameplay | game-designer | S |
| 8 | Narrative Discovery System | MVP | Narrative | game-designer, narrative-director | M |
| 9 | Diegetic UI System | MVP | UI | ux-designer, game-designer | M |
| 10 | Player Visual System | MVP | Presentation | art-director, technical-artist | M |
| 11 | Save/Load System | MVP | Persistence | systems-designer | M |
| 12 | Environmental Hazard System | VS | Gameplay | game-designer | M |
| 13 | Metroidvania Progression | VS | Gameplay | game-designer, systems-designer | M |
| 14 | Creature Visual System | VS | Presentation | art-director, technical-artist | M |
| 15 | World Rendering Pipeline | VS | Presentation | technical-artist | M |
| 16 | Adaptive Audio System | VS | Audio | sound-designer | M |
| 17 | Decay & Succession System | Alpha | Gameplay | systems-designer | M |
| 18 | Settings & Accessibility | Alpha | Meta | ux-designer | S |

**Architecture specs** (not full GDDs — designed during `/create-architecture`):

| System | Priority | Layer | Notes |
|--------|----------|-------|-------|
| Input System | MVP | Foundation | Standard input mapping — Godot InputMap + action definitions |
| Scene Management | MVP | Foundation | Biome streaming strategy — architecture decision needed |
| Game State Machine | MVP | Foundation | State transitions — standard FSM pattern |
| Camera System | MVP | Core | 2D platformer camera with parallax — architecture decision for screen transitions |

**Effort key**: S = 1 session, M = 2-3 sessions, L = 4+ sessions. A "session" is one focused design conversation producing a complete GDD.

---

## Circular Dependencies

None found. All dependency chains are acyclic:

- Creature AI → Population & Spawning → (no return dependency)
- Biological Evolution → Metroidvania Progression → (no return dependency)
- Narrative Discovery → Decay & Succession → (no return dependency)
- Weather → Creature AI → Population → (no return to Weather)

Potential concern: **Creature AI ↔ Population & Spawning** could become circular if population rules require AI behavior data to determine spawn viability. Resolution: Population system defines *what species exist and where*; AI system defines *how individuals behave after spawning*. Population reads species roster data, not live AI state. The boundary is clean.

---

## High-Risk Systems

| System | Risk Type | Risk Description | Mitigation |
|--------|-----------|-----------------|------------|
| **Creature AI System** | Technical + Design | Must be complex enough to feel alive (Pillar 1) but readable enough to learn through observation (Pillar 2). GDScript performance at scale is uncertain. This system IS the game — if it fails, nothing else matters. | Prototype first (`/prototype ecological-ai`). Define "good enough" behavioral complexity floor. Performance profile early — GDExtension fallback for AI hot paths if needed. |
| **Biological Evolution System** | Design | Permanent path-dependent mutations with no respec (Pillar 4). Must prevent soft-locks where an evolution path cannot complete the critical path. The evolution tree must be balanced without a respec safety net. | Automated graph validator ensuring every evolution path reaches every required area. Fallback: "universal" mutations that open blocked paths without breaking evolution identity. |
| **Population & Spawning (Simulation LOD)** | Technical | Two simulation paradigms (full AI vs. statistical models) must produce consistent world states. Statistical models must be accurate enough that transitioning a biome from statistical → full simulation doesn't produce jarring population shifts. | Define clear boundary contract between full and statistical simulation. Test LOD transitions extensively. Start with simple statistical model, add fidelity as needed. |
| **Diegetic UI System** | Design | No HUD means ALL information must be conveyed through the world and creature sprite. If health readability fails, the game becomes unplayable. If navigation fails, players get lost without recourse (Anti-pillar: NOT hand-holdy means no fallback minimap). | Art bible has detailed sprite degradation tiers. Prototype health communication early. The dorsal ridge compass is the one non-diegetic exception — validates that some concession to readability is built in. |
| **Metroidvania Progression** | Design | Biggest open question from concept: "How does gating work when creatures don't guard doors and abilities are path-dependent?" Ecological gates are a novel design pattern with no proven precedent. | Defer to Vertical Slice (not MVP). Prototype 2-3 gate types before full design. Study Rain World's region gating for precedent. Accept that this system may require multiple design iterations. |

---

## Progress Tracker

| Metric | Count |
|--------|-------|
| Total systems identified | 22 |
| GDD-worthy systems | 18 |
| Architecture spec systems | 4 |
| Design docs started | 0 |
| Design docs reviewed | 0 |
| Design docs approved | 0 |
| MVP systems total | 15 |
| MVP GDDs to write | 11 |
| Vertical Slice systems | 5 |
| Alpha systems | 2 |

---

## Next Steps

- [ ] Design MVP-tier systems in order (use `/design-system [system-name]` or `/map-systems next`)
- [ ] Run `/design-review` on each completed GDD
- [ ] Run `/review-all-gdds` after all MVP GDDs are complete
- [ ] Run `/gate-check pre-production` when MVP systems are designed
- [ ] Prototype highest-risk system early (`/prototype ecological-ai`)
- [ ] Run `/create-architecture` after MVP GDDs are approved — architecture flows from design, not the reverse
