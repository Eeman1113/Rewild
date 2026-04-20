# Rewild

**Nature never destroys. It digests.**

---

## The Pitch

An ecological Metroidvania where you play as an evolved creature navigating a post-human world ruled by ecological law -- every organism hunts, hides, and migrates for real reasons, and you're not at the top of the food chain. Humanity devolved from AI dependency, nature reclaimed civilization, and now the ruins of human progress are being slowly digested by an ecosystem that doesn't know you exist.

---

## The Fantasy

You are a small thing in a vast, indifferent world that was never built for you -- and mastering it means understanding it.

The world moved on without humanity. Trees split concrete. Rain floods server farms. Creatures have evolved into forms no human would recognize. You are one of these creatures -- not special, not chosen, just alive. The world does not care if you survive. It does not care if you understand what happened to the beings who built the crumbling structures around you. But if you watch carefully, if you learn the rhythms of weather and migration and predation, if you survive long enough to evolve -- you will discover truths buried in the ruins that change what it means to be alive.

This is not a power fantasy. This is the fantasy of *belonging* to something vast and indifferent and beautiful, and earning your place in it through understanding.

---

## Game Pillars

**The World Doesn't Care About You** -- The ecosystem runs on its own logic. Weather, migration, predation, decay. You are not the protagonist of this world. You are a participant. Every creature exists for its own reasons, not to challenge you.

**Mastery Through Understanding** -- Difficulty comes from the world being complex, not from artificial spikes. The player who observes, learns, and adapts will thrive. The player who brute-forces will die repeatedly. Knowledge *is* the progression system.

**Every Ruin Tells a Truth** -- Human civilization's remains are the narrative delivery system. Each ruin reveals a fragment of how humanity fell: the cognitive decline, the AI dependency, the slow devolution. The story is archaeological, not scripted.

**You Become What You Survive** -- The creature evolves based on what environments it endures. Progression isn't a skill tree -- it's a biological record of the journey. Two players who took different paths will have different creatures. Evolution is permanent. There is no respec.

---

## Visual Identity: "Quiet Takeover"

Organic forms consuming geometric ones -- but never replacing them. The geometry of human architecture is always present as a skeleton, and biological shapes grow through it, wrap around it, deform it.

- **Color is a clock.** The palette shifts by biome based on ecological succession. Fresh ruins retain human color (concrete grays, rusted steel oranges). Ancient ruins are almost entirely biological (deep chlorophyll greens, bioluminescent teals). Players learn to read time by how much human color remains.
- **Shape tells you what something eats.** The player creature is small, soft-bodied, rounded. Predators inherit the sharpness of the ruins they nest in. Prey inherits the softness of the moss they hide within. Silhouettes communicate ecological role at any zoom level.
- **The human past is always visible.** Ruins are never fully erased. Every environment shows the skeleton of human infrastructure beneath biological growth. The world is a palimpsest -- the new written over the old, but the old still legible.

2D pixel art. 480x270 viewport. Diegetic UI only -- no HUD, no health bars, no minimap. Health is communicated through sprite degradation. The camera frames the player like a nature documentary frames a small animal in a large landscape.

---

## Technical Stack

| Component | Choice |
|-----------|--------|
| Engine | Godot 4.6 |
| Primary Language | GDScript |
| Performance Paths | C# or GDExtension (C++) for AI hot paths |
| Viewport | 480x270 native, integer-scaled |
| Platform | Mac and PC (Steam) |
| Monetization | Premium, buy once |
| Networking | None -- single-player |

---

## Current Status

**Phase: Pre-production / Design**

The project is in the design documentation phase. Core systems are being specified before implementation begins. The game concept, art bible, player controller, and systems index are complete and reviewed. Remaining design documents are in progress.

What exists:
- Game concept document with full MDA analysis, core loop definition, player profile, and scope tiers
- Art bible with complete visual identity specification, mood targets for all game states, and per-element art direction
- Player controller design with full physics specification, state machine, and feel layer system
- Systems index enumerating all 22 required systems with priority tiers and dependency mapping

What's next:
- Complete remaining design documents (creature AI, biome design, evolution system, narrative bible, audio design)
- Technical architecture specification
- Prototype the riskiest system (ecological AI)
- First playable MVP targeting 3 biomes, 8-10 creatures, core loop validation

---

## Documentation Index

| Document | Description |
|----------|-------------|
| [README.md](README.md) | Project overview, vision, and documentation index (this file) |
| [game-concept.md](game-concept.md) | Complete game concept: elevator pitch, core fantasy, MDA analysis, core loop, game pillars, player profile, scope tiers, MVP definition, narrative foundation, risks and open questions |
| [art-bible.md](art-bible.md) | Visual identity specification: "Quiet Takeover" direction, three supporting visual principles (Decay Gradient, Silhouette Ethology, The Uninvited Frame), mood and atmosphere targets for all game states, per-element art direction |
| [systems-index.md](systems-index.md) | Master index of all 22 game systems across 9 categories, with priority tiers (MVP/Vertical Slice/Alpha/Full Vision), dependency mapping, and rationale for each system's priority |
| [player-controller.md](player-controller.md) | Player movement system design: physics foundation, ground movement, jumping, wall interactions, climbing, crawling, swimming, feel layer system, state machine, evolution interface |
| [narrative-bible.md](narrative-bible.md) | World lore, humanity's fall, archaeological storytelling structure, ruin fragment catalog, narrative arc design |
| [creature-design.md](creature-design.md) | Creature AI architecture, behavioral reasoning system, species catalog, food web design, predator/prey dynamics |
| [biome-design.md](biome-design.md) | Biome specifications, ecological succession gradient, interconnection design, environmental hazards, tileset requirements |
| [evolution-system.md](evolution-system.md) | Biological evolution system: mutation triggers, evolution branches, soft-lock prevention, visual progression, critical path validation |
| [weather-ecology.md](weather-ecology.md) | Weather simulation, ecological impact on creature behavior and traversal, seasonal cycles, weather-driven gameplay dynamics |
| [audio-design.md](audio-design.md) | Adaptive audio architecture, ecosystem soundscaping, creature vocalizations, weather audio, minimal music philosophy |
| [diegetic-ui.md](diegetic-ui.md) | Diegetic UI specification: no-HUD design, health through sprite degradation, environmental wayfinding, in-world information delivery |
| [technical-architecture.md](technical-architecture.md) | Engine architecture, simulation LOD strategy, save data tiers, performance budgets, scene management |
| [mvp-roadmap.md](mvp-roadmap.md) | Development roadmap: MVP through release scope tiers, milestone definitions, critical path analysis, risk assessment, quality gates |
| [brainstorming.md](brainstorming.md) | Working scratchpad for ideas, experiments, and design explorations not yet promoted to formal documents |
| [references-and-inspirations.md](references-and-inspirations.md) | Detailed analysis of reference games and non-game inspirations, what to take, what to leave, and why each reference matters |

---

## Inspirations

**Games:**
- **Rain World** -- Ecological indifference, creature AI with real behaviors, hostile world that doesn't revolve around the player
- **Hollow Knight** -- Atmospheric Metroidvania world design, interconnected biomes, environmental storytelling, precise combat
- **Celeste** -- Precision platforming feel, tight controls that reward mastery, difficulty as emotional expression
- **Stray** -- Post-human world from non-human perspective, environmental storytelling of collapsed civilization
- **Death Stranding** -- Lonely traversal through beautiful emptiness, connection through isolation, the world as the antagonist
- **Outer Wilds** -- Knowledge as the only progression, archaeological discovery, pure curiosity-driven engagement

**Non-game:**
- The Chernobyl Exclusion Zone wildlife recovery -- nature reclaiming human spaces with startling speed
- Alan Weisman's *The World Without Us* -- how nature would reclaim human civilization
- David Attenborough documentaries -- creature behavior as endlessly fascinating when observed with patience
- Nicholas Carr's *The Shallows* -- how technology reshapes human cognition, taken to its extreme conclusion

---

## Scope

| Tier | Content | Timeline |
|------|---------|----------|
| **MVP** | 3 biomes, 8-10 creatures, core loop, basic weather, one evolution path | 3-4 months |
| **Vertical Slice** | 5 biomes, 15-20 creatures, migration, full evolution tree, one narrative arc | 6-8 months |
| **Alpha** | 10 biomes, 30+ creatures, seasonal cycles, ecosystem persistence, full narrative | 12-16 months |
| **Full Vision** | 16+ biomes, 50+ creatures, procedural elements, complete world-state, polish | 18-24 months |

---

## Contact

Solo development project. Built with Godot 4.6 and GDScript.
