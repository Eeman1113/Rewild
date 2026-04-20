# Rewild -- Weather & Ecological Simulation Design

This document specifies how the weather system and ecological simulation work together to create a living, responsive world. Nothing in this system is cosmetic. Every element affects gameplay, creature behavior, and the player's survival.

---

## 1. Weather System Design

### Core Philosophy

Weather in Rewild is an ecological force, not an aesthetic filter. Rain isn't a visual effect -- it's a hydrological event that raises water levels, drives creatures to shelter, changes the acoustic landscape, triggers plant growth, and creates traversal hazards. Every weather state changes the rules of the world.

The player should learn to read the weather the way a real animal does: by watching the sky, feeling the wind, observing creature behavior. When birds go silent and the wind shifts, a storm is coming. This is learnable, diegetic, and survival-critical.

### Weather Types

#### Clear
- **Visual:** Full sun, sharp shadows, high visibility. Blue sky with procedural cloud wisps.
- **Gameplay effect:** Maximum visibility for both player and predators. Highest predation risk in open areas (you're visible, predators are active). Best conditions for observation and exploration. Thermal updrafts available for gliding in open areas.
- **Creature behavior:** Diurnal predators most active. Prey species more cautious in open ground, more active in cover. Basking behavior for ectotherms. Maximum foraging range for all species.
- **Sound:** Full ambient creature chorus. Insect activity high. Wind minimal.
- **Duration:** 2-6 in-game hours.

#### Overcast
- **Visual:** Diffuse light, soft shadows, muted colors. Cloud layer obscures sun. Slightly reduced visibility at distance.
- **Gameplay effect:** Reduced predation risk in open areas (less sharp visual contrast). Good exploration conditions. Moderate traversal difficulty. Temperature drop affects ectotherm activity.
- **Creature behavior:** General reduction in basking. Slight increase in small prey activity (less predation pressure from visual predators). Ambush predators more effective (less shadow contrast to reveal their position).
- **Sound:** Creature chorus slightly muted. Wind audible. Atmospheric pressure change detectable (if evolved).
- **Duration:** 3-8 in-game hours.

#### Rain (Light)
- **Visual:** Particle rain, water collecting on surfaces, puddle formation. Visibility moderately reduced. Wet surface reflections.
- **Gameplay effect:** Surface traction reduced (slippery traversal on stone, metal). Water levels in streams and pools begin rising. Sound masking (rain noise covers player movement but also covers predator approach). Scent trails degrade faster.
- **Creature behavior:** Many species seek partial shelter. Aquatic and semi-aquatic species more active. Insect activity reduced. Amphibian species emerge. Worm-eating birds active.
- **Sound:** Rain on different surfaces (metal, stone, leaves, water, soil, glass) each has a distinct sound. This is critical for audio-based navigation. The rain soundscape tells you what materials are around you.
- **Duration:** 1-4 in-game hours.

#### Rain (Heavy)
- **Visual:** Dense particle rain, reduced visibility to medium range. Flowing water on surfaces. Active drainage channels. Water level rising significantly in low areas.
- **Gameplay effect:** Severe traction reduction. Some low passages begin flooding. Strong sound masking (very difficult to hear predator approach, but equally difficult for predators to hear you). Scent trails eliminated. Rivers and streams become impassable without aquatic adaptations. Some shelter entrances may flood.
- **Creature behavior:** Most terrestrial species sheltering. Aquatic species highly active. Semi-aquatic predators (crocodilian analogues) at peak activity. Burrowing species may be flushed from underground. Fishing opportunities for adapted creatures.
- **Sound:** Dominant rain noise. Individual surfaces still distinguishable but require attention. Water flow sounds from drainage channels, streams, waterfalls becoming more intense.
- **Duration:** 1-3 in-game hours.

#### Storm
- **Visual:** Extreme particle effects. Lightning illumination (brief, dramatic). Very low visibility. Trees and vegetation moving violently. Debris in wind. Darkened sky.
- **Gameplay effect:** Dangerous traversal. Lightning strike risk in exposed areas (rare but lethal). Wind affects movement (pushed laterally, gliding becomes impossible or extremely risky). Flooding in low areas. Some areas become temporarily inaccessible. Shelter is critical -- being caught in the open during a storm is a survival crisis. However: storms create unique opportunities. Storm currents enable weather-based fast travel for evolved gliders. Lightning strikes can start fires (see fire ecology). Electromagnetic storms near server farms trigger unique evolutionary pressures.
- **Creature behavior:** Nearly all species sheltering. Extreme-weather adapted species (storm petrel analogues) active. Predator-prey truce in shared shelters (thermal spa effect). Post-storm emergence creates brief period of intense activity across all species.
- **Sound:** Thunder, wind, rain, structural creaking, debris impacts. The most intense soundscape in the game. Inside a shelter, the storm is muffled but present -- one of the game's signature atmospheric moments.
- **Duration:** 0.5-2 in-game hours. Storms are rare and impactful.

#### Wind (no precipitation)
- **Visual:** Vegetation movement, particle debris (leaves, dust, pollen), directional indicators. Visibility affected by airborne particles in dry biomes (dust/sand).
- **Gameplay effect:** Movement affected directionally (headwind slows, tailwind accelerates, crosswind pushes). Scent detection enhanced downwind, eliminated upwind (critical for predator detection). Gliding strongly affected (wind direction determines viable glide routes). Sound carries differently (downwind sounds reach further).
- **Sound:** Wind is the dominant sound element. It interacts with structures (whistling through ruins, moaning through tunnels). Creature calls carry on the wind or are lost to it.
- **Duration:** 2-6 in-game hours.

#### Fog / Mist
- **Visual:** Drastically reduced visibility. Close objects clear, medium objects hazy, distant objects invisible. Moisture on surfaces. Volumetric light shafts where sun penetrates.
- **Gameplay effect:** Extremely reduced visual range for both player and creatures. Echolocation becomes critical (if evolved). Scent enhanced (moist air carries scent further). Sound slightly muffled but directional. Predators relying on vision are less effective. Predators relying on other senses (echolocation, scent, tremor) are MORE effective. One of the most atmospheric and tense weather states. Fog creates a fundamentally different game -- intimate, claustrophobic, beautiful.
- **Creature behavior:** Visual predators less active. Sensory predators more active. Prey species in mixed state -- reduced visual predation risk but increased ambush risk. Many species reduce movement (can't assess environment).
- **Sound:** Muffled, intimate, close. Sounds seem to come from everywhere and nowhere. Creature calls are distorted. Water dripping is the dominant close-range sound.
- **Duration:** 2-5 in-game hours. Most common in early morning, coastal biomes, and low-lying areas.

### Weather Transitions

Weather never switches instantaneously. Every transition is gradual, providing readable warning signs.

**Transition indicators (in approximate order of appearance):**
1. **Wind direction change** (10-15 minutes before weather shift). Wind shifts are the earliest weather signal. Animals that can read wind have the most advance warning.
2. **Cloud formation/dissipation** (8-12 minutes before precipitation change). Clouds build gradually, thicken, lower. Or thin, break, dissipate.
3. **Atmospheric pressure change** (5-10 minutes before). Detectable only with evolved pressure sensing. Some creatures have this naturally and change behavior, providing secondary indicators.
4. **Creature behavior change** (5-8 minutes before). Birds stop singing before rain. Insects go quiet before storms. Fish surface-feed before pressure drops. These are the most accessible weather indicators for the player.
5. **Temperature change** (3-5 minutes before). Subtle visual cues (breath visibility, surface condensation).
6. **First precipitation/clearing** (0 minutes). The weather state arrives.

**Transition duration:** 5-15 minutes depending on severity of change. Clear to storm has the longest transition. Rain to heavy rain is faster. Fog onset can be extremely rapid (2-3 minutes in coastal areas).

### Per-Biome Weather Probability Tables

Each biome has a weather probability distribution that changes with time of day and season (if implemented).

#### Temperate Forest
| Weather | Dawn | Day | Dusk | Night |
|---------|------|-----|------|-------|
| Clear | 25% | 35% | 30% | 20% |
| Overcast | 30% | 25% | 25% | 30% |
| Light Rain | 20% | 15% | 20% | 25% |
| Heavy Rain | 10% | 10% | 10% | 15% |
| Storm | 3% | 5% | 5% | 3% |
| Wind | 7% | 8% | 7% | 5% |
| Fog | 5% | 2% | 3% | 2% |

#### Coastal / Tidal
| Weather | Dawn | Day | Dusk | Night |
|---------|------|-----|------|-------|
| Clear | 15% | 25% | 20% | 15% |
| Overcast | 25% | 25% | 25% | 25% |
| Light Rain | 20% | 15% | 20% | 20% |
| Heavy Rain | 15% | 10% | 15% | 15% |
| Storm | 5% | 8% | 5% | 5% |
| Wind | 10% | 12% | 10% | 10% |
| Fog | 10% | 5% | 5% | 10% |

#### Underground / Subterranean
| Weather | All times |
|---------|-----------|
| Stable | 70% |
| Flooding (surface rain) | 15% |
| Condensation fog | 10% |
| Seismic activity | 5% |

Underground biomes are sheltered from surface weather but respond to it indirectly. Heavy surface rain causes underground flooding on delay (15-30 minutes after surface rain begins). Seismic events are independent.

#### Ruined Urban
| Weather | Dawn | Day | Dusk | Night |
|---------|------|-----|------|-------|
| Clear | 20% | 30% | 25% | 15% |
| Overcast | 30% | 25% | 30% | 30% |
| Light Rain | 20% | 15% | 15% | 25% |
| Heavy Rain | 10% | 10% | 10% | 15% |
| Storm | 5% | 8% | 8% | 5% |
| Wind | 10% | 10% | 10% | 8% |
| Fog | 5% | 2% | 2% | 2% |

Urban biomes have wind tunnel effects (narrow streets amplify wind) and heat island remnants (concrete retains heat, creating micro-weather patterns).

#### Wetland / Marsh
| Weather | Dawn | Day | Dusk | Night |
|---------|------|-----|------|-------|
| Clear | 10% | 20% | 15% | 10% |
| Overcast | 25% | 25% | 25% | 25% |
| Light Rain | 25% | 20% | 25% | 25% |
| Heavy Rain | 15% | 10% | 15% | 15% |
| Storm | 3% | 5% | 3% | 3% |
| Wind | 5% | 8% | 5% | 5% |
| Fog | 17% | 12% | 12% | 17% |

Wetlands have the highest fog probability and overall moisture. Water levels fluctuate significantly with rain.

#### Server Farm / Tech Ruins
| Weather | All times |
|---------|-----------|
| Interior stable | 60% |
| Condensation (from cooling systems) | 20% |
| Electromagnetic interference | 10% |
| Steam venting | 10% |

Server farms have their own micro-weather driven by still-running machinery. EM interference affects evolved sensory systems (echolocation garbled, scent vision distorted). Steam venting creates temporary thermal updrafts and visibility reduction.

### Day/Night Cycle

The day/night cycle runs on a fixed schedule independent of weather. One full cycle = approximately 40 real-time minutes (adjustable during testing).

| Phase | Duration | Description |
|-------|----------|-------------|
| Dawn | 5 min | Light increasing, nocturnal creatures retiring, diurnal creatures waking. Transition period with maximum species overlap. Fog most likely. |
| Day | 15 min | Full light. Diurnal species active. Highest visibility. |
| Dusk | 5 min | Light decreasing, diurnal creatures retiring, nocturnal creatures waking. Second transition overlap. |
| Night | 15 min | Darkness. Nocturnal species active. Visibility severely reduced without evolved night vision or echolocation. Bioluminescent organisms visible. |

Dawn and dusk are the most ecologically active periods -- the overlap between diurnal and nocturnal species creates the highest biodiversity windows and the highest predation risk.

Night transforms the game. Without evolved night vision, the player is effectively blind beyond a short range. Echolocation renders the world as sound-waves. Bioluminescence provides natural landmarks. Nocturnal predators are faster, quieter, and more numerous than their diurnal counterparts. Night is not a cosmetic darkness filter -- it is a fundamentally different game state.

---

## 2. Ecological Simulation

### Core Philosophy

The ecosystem is the game. Not a backdrop, not a setting, not an aesthetic layer -- the functional heart of the play experience. Every creature is a participant in a real (simulated) ecology. Energy flows from sun to plant to herbivore to predator. Populations rise and fall. Territories shift. Migrations happen. The world is alive in the way that matters: it functions.

The player is part of this ecosystem. Not above it, not outside it -- embedded in it. The creature eats, is eaten, competes, cooperates, and dies within the same system that governs every other organism. The player's special status comes only from having a human intelligence directing a creature body -- the advantage of understanding, not the advantage of power.

### Food Web Dynamics

#### Energy Flow
Energy enters the ecosystem through primary producers:
- **Photosynthetic plants:** Aboveground vegetation. Energy source depends on sunlight (reduced during overcast/storm, absent at night, irrelevant underground).
- **Chemosynthetic organisms:** Around geothermal vents, in deep underground biomes, near still-running server farm heat sources. These organisms don't need sunlight and provide energy to ecosystems that exist in permanent darkness.
- **Decomposers:** Fungi, bacteria, and detritivores break down dead organic matter and return nutrients to the soil, completing the cycle.

Energy flows through trophic levels:
1. **Producers** (plants, algae, chemosynthetic bacteria)
2. **Primary consumers** (herbivores, detritivores)
3. **Secondary consumers** (small predators, omnivores)
4. **Tertiary consumers** (apex predators)
5. **Decomposers** (fungi, bacteria -- returning energy to level 1)

Each level transfer is approximately 10% efficient (the 10% rule from real ecology). This means apex predator populations are naturally small. A biome that supports 1000 units of plant life supports ~100 units of herbivore, ~10 units of small predator, and ~1 unit of apex predator. These ratios drive the population balance of the simulation.

**Player position in the food web:** Variable based on evolution. Early game: primary consumer or low secondary consumer. Late game: potentially secondary or tertiary consumer in some biomes. The player's trophic level determines what eats them and what they can eat.

#### Nutrient Cycling
- Dead organisms decompose, returning nutrients to the soil.
- Plants grow faster in nutrient-rich soil (near decomposition events, in fertile biomes).
- Water carries nutrients downstream (riparian areas are high-productivity zones).
- Fungal networks distribute nutrients underground between connected organisms.
- Player actions affect nutrient flow: killing many creatures in one area creates a nutrient hotspot. Avoiding an area allows it to develop nutrient-poor conditions.

### Population Dynamics

#### Birth and Death
Every species has:
- **Birth rate:** Influenced by food availability, season, shelter availability, population density. Species reproduce when conditions are favorable. Some species are seasonal breeders, others opportunistic.
- **Death rate:** Influenced by predation, starvation, disease, environmental hazards, old age. Death is the primary balancing mechanism.
- **Carrying capacity:** The maximum population a biome can support for each species. Determined by food availability, shelter availability, and territory size. When population exceeds carrying capacity, death rate increases (starvation, increased predation due to overcrowding, disease).

Population changes are calculated per biome at regular intervals (every few in-game minutes for nearby biomes, less frequently for distant ones).

#### Lotka-Volterra Dynamics (Predator-Prey Oscillation)
The classic predator-prey oscillation drives population dynamics:
1. Prey population is high -> predators have abundant food -> predator population increases
2. Predator population is high -> prey is over-hunted -> prey population decreases
3. Prey population is low -> predators starve -> predator population decreases
4. Predator population is low -> prey has reduced predation -> prey population increases
5. Return to step 1

This cycle creates natural rhythms in the ecosystem. A biome recently depleted of predators (perhaps the player killed one) will experience a prey boom, followed by vegetation depletion, followed by prey crash, followed by slow predator recovery. These cascades take in-game days to play out, but they're visible if the player is paying attention.

**Dampening factors:** Real ecosystems have mechanisms that prevent extreme oscillation:
- Predator switching (predators shift to alternative prey when primary prey is scarce)
- Prey refugia (areas where prey is safe from predation, maintaining a breeding population)
- Density-dependent effects (crowded populations are more vulnerable to disease)
- Carrying capacity limits (populations can't grow beyond what the food supply supports)

#### Migration
Species migrate for predictable ecological reasons:
- **Seasonal migration:** Moving to follow food sources or avoid harsh weather. Large-scale, predictable, periodic.
- **Resource depletion migration:** When a local food source is exhausted, herbivores move to new feeding grounds. Predators follow.
- **Breeding migration:** Some species travel to specific locations to breed (salmon-analogue returning to birth streams, etc.).
- **Disturbance migration:** After a fire, flood, or other disturbance, species relocate from affected areas.
- **Player-driven migration:** If the player significantly disrupts a local ecosystem, species may relocate away from the disturbance.

Migration creates dynamic content: a biome that was empty yesterday might be full of migrating creatures today. A usually-safe passage might be blocked by a moving herd. Migration is both spectacle and gameplay.

### Territorial Behavior

Territory is fundamental to how creatures use space.

#### Territory Types
- **Exclusive territory:** One individual or mating pair controls an area. Intruders are confronted and expelled. Apex predators typically hold exclusive territories.
- **Shared territory:** A group (herd, pack, colony) controls an area collectively. Defense is communal.
- **Overlapping territories:** Multiple individuals' territories overlap. Encounters in overlap zones are governed by dominance relationships.
- **No territory:** Some species are nomadic, following resources without claiming space.

#### Territory Mechanics
- **Scent marking:** Territorial creatures mark boundaries with scent. The player can detect these with evolved olfaction. Entering a marked territory triggers territorial defense behavior from the owner.
- **Visual displays:** Some creatures use visual signals (color changes, posturing, size displays) to assert territory. These are readable by the player.
- **Acoustic marking:** Some species use calls to define territory. The "soundscape" of a biome includes territorial calls that map the invisible boundaries.
- **Territory size:** Proportional to the organism's energy needs and food density. In resource-rich biomes, territories are small. In resource-poor biomes, territories are large.

**Player interaction with territory:** The player creature can be treated as a territorial intruder, a potential prey item, a non-threat (too small to matter), or a competitor, depending on the territory-holder's species and the player's current size/evolution state.

### Ecological Balance

The ecosystem maintains balance through interconnected feedback loops:

- **Top-down control:** Predators control prey populations, which controls vegetation. Remove a predator and the cascade flows down.
- **Bottom-up control:** Primary productivity (plant growth) determines how much life a biome can support. Drought reduces plant growth, which reduces herbivore populations, which reduces predator populations.
- **Lateral effects:** Competition between species at the same trophic level. Two herbivore species competing for the same food source will limit each other's populations.
- **Keystone species:** Certain species have disproportionate ecological impact. Removing a keystone species causes cascading effects throughout the ecosystem. The player may unknowingly encounter and affect keystone species.
- **Ecosystem engineers:** Species that physically modify the environment (dam-builders, burrowers, coral-analogues). Their constructions create habitat for other species. Destroying their structures affects entire communities.

---

## 3. Simulation LOD (Level of Detail)

Running a full ecological simulation for the entire game world simultaneously is computationally impossible. The simulation operates at different fidelity levels depending on proximity to the player.

### LOD Tiers

#### Tier 1: Current Biome (Full Simulation)
- **Range:** The biome the player currently occupies, plus a buffer zone into adjacent biomes.
- **Fidelity:** Individual creature AI. Every creature is a discrete entity with position, state, needs, behaviors, and decision-making. Full pathfinding, full sensory modeling, full interaction capability.
- **Update rate:** Every frame (creature rendering and input response) with AI decisions updated every 0.1-0.5 seconds depending on creature type and state.
- **Creature count:** Target 30-80 active creatures in the player's immediate area, with more in the broader biome zone managed at slightly reduced fidelity.
- **What's simulated:** Individual creature position and movement, hunting/fleeing/foraging behavior, territorial interactions, parent-offspring interactions, predator-prey encounters, response to weather, response to player, reproductive behavior, death and decomposition.

#### Tier 2: Adjacent Biomes (Simplified Simulation)
- **Range:** Biomes directly connected to the player's current biome.
- **Fidelity:** Population-level statistics with representative individual agents. The biome knows: total population per species, general health of populations, current weather state, recent significant events (large predator death, migration arrival, etc.). A small number of representative creatures are tracked individually to handle cross-boundary events.
- **Update rate:** Every 30-60 seconds.
- **What's simulated:** Population growth/decline, migration triggers, major ecological events, weather state. Individual creature behavior is NOT simulated -- only population statistics.

#### Tier 3: Distant Biomes (Statistical Model)
- **Range:** All biomes not in Tier 1 or Tier 2.
- **Fidelity:** Pure population statistics. Each biome is a set of numbers: population per species, food availability, carrying capacity. Simple equations update these numbers periodically.
- **Update rate:** Every 5-10 minutes of real time.
- **What's simulated:** Population trends, carrying capacity changes, seasonal effects. No individual creatures, no specific events, no spatial information.

### LOD Transitions

The critical challenge is making the transition between LOD tiers seamless. When the player moves from one biome to another, the adjacent biome must "spin up" from Tier 2 to Tier 1, and the previous biome must "spin down" from Tier 1 to Tier 2.

#### Spin-Up Process (Tier 2 -> Tier 1)
1. **Population seeding:** Based on Tier 2 population statistics, individual creatures are spawned at ecologically appropriate locations (near food sources, in shelters, along paths). This uses pre-defined spawn probability maps per species per biome.
2. **State initialization:** Each spawned creature is given a behavioral state appropriate to the current time, weather, and ecological conditions. A predator might be spawned mid-hunt if it's hunting time. A prey species might be spawned near water if it's drinking time.
3. **Gradual reveal:** Creatures appear at the edges of the player's perception first, moving inward. This prevents "pop-in" -- creatures seem to walk into view rather than appearing.
4. **Memory integration:** If the player has visited this biome before, creature placement reflects the previous state as much as possible. If the player killed a specific predator, that predator is absent. If the player disrupted a nest, the nest is relocated.

#### Spin-Down Process (Tier 1 -> Tier 2)
1. **Population census:** The current state of all individual creatures is aggregated into population statistics. Total count per species, health distribution, recent births/deaths.
2. **Event logging:** Significant events (predator kills, nest locations, territorial boundaries) are logged for recall if the player returns.
3. **Creature persistence:** A small number of "important" creatures (those the player has interacted with, bonded with, or that are ecologically significant) are tracked individually even in Tier 2, so they're present if the player returns.
4. **Gradual fade:** As the biome moves out of Tier 1 range, individual creatures are removed from the edges inward. They don't disappear -- they move out of view.

#### Boundary Handling
Biome boundaries are permeable. Creatures cross between biomes based on need (food, shelter, territory). At the boundary between a Tier 1 and Tier 2 biome:
- Creatures in the Tier 1 biome that move toward the boundary enter a "transition zone" where they can cross into the Tier 2 biome (becoming statistical) or turn back.
- Tier 2 population statistics can generate individual creatures that "arrive" from the adjacent biome, appearing at the boundary and moving into the Tier 1 area.
- This creates the illusion of a continuous ecosystem extending beyond the player's immediate view.

### Performance Budget

Target platform: not yet determined, but assume moderate hardware. The ecological simulation competes with rendering, audio, physics, and game logic for processing time.

**Budget allocation (per frame at 60fps):**
- Rendering: 8ms
- Creature AI (Tier 1): 3ms
- Physics and collision: 2ms
- Audio: 1ms
- Weather and ecology updates: 0.5ms (amortized -- heavy updates run on a timer, not every frame)
- Game logic, input, UI: 1ms
- Buffer: 1.17ms

**Optimization strategies:**
- AI decisions are staggered (not all creatures decide on the same frame)
- Spatial partitioning (only creatures near the player get full pathfinding)
- Behavior caching (a creature in a stable state doesn't need frequent re-evaluation)
- LOD transitions run over multiple frames (spin-up is gradual, not instantaneous)
- Tier 2 and Tier 3 updates run on a coroutine/thread, not blocking the main game loop

---

## 4. Ecological Events

Events that emerge from the simulation or are triggered by specific conditions. These are not scripted setpieces -- they're systemic outcomes that create dynamic content.

### Predator Kill Events
When a predator kills prey in the player's vicinity:
1. **Kill occurs:** Visual and audio indicators (predator attack animation, prey distress call, brief chase or ambush sequence).
2. **Feeding:** Predator feeds on the kill. Duration depends on predator size and hunger level.
3. **Scavenger attraction:** Smaller scavenger species detect the kill (via scent, visual cues, or observing the predator's behavior) and begin approaching. They wait at a distance until the predator finishes or leaves.
4. **Scavenger feeding:** Multiple scavenger species feed, with dominance hierarchies determining feeding order.
5. **Insect colonization:** After larger scavengers leave, insects colonize the remains.
6. **Decomposition:** Over in-game hours/days, the carcass decomposes, enriching the soil. Plants in the area grow faster for a period.

**Player interaction:** The player can scavenge from kills (if they can compete with other scavengers). The player can use a predator's focus on its kill to pass through its territory unnoticed. The player can follow scavengers to find kill sites.

### Weather-Driven Behavior Changes
Weather events cause cascading behavior changes across all species:

**Rain onset:**
- Terrestrial herbivores move toward shelter or dense vegetation
- Amphibian species emerge and become active
- Aquatic species' accessible range expands as water levels rise
- Insectivores become active (rain flushes insects)
- Burrowing species may be flooded out of underground shelters
- Scent-tracking predators lose effectiveness (rain degrades scent trails)

**Storm:**
- Nearly all species seek shelter immediately
- Predator-prey dynamics pause (both are sheltering, often in proximity -- the "thermal spa" effect)
- Post-storm: intense activity burst as all species emerge simultaneously. High predation risk during post-storm emergence (hungry predators, disoriented prey).

**Fog:**
- Visual predators reduce activity
- Ambush predators increase activity (visibility reduction benefits ambush strategy)
- Prey species in mixed behavioral state (reduced visual predation risk, increased ambush risk)
- Acoustic activity increases (creatures use sound more when they can't see)

### Population Events

#### Population Boom
Triggered when: a predator is removed from the ecosystem (killed by player, died of other causes, migrated away), AND prey species are near carrying capacity.
**Effect:** Prey population surges. Vegetation is overgrazed. Other herbivore species are outcompeted. Secondary effects cascade outward.
**Duration:** Days to weeks of in-game time before balance restores.
**Player experience:** A biome the player visited before is visibly different. Creatures are everywhere. Vegetation is stripped. The player's actions had consequences.

#### Population Crash
Triggered when: a population exceeds carrying capacity, disease spreads, or a critical resource disappears.
**Effect:** Mass mortality. Carcasses attract scavengers. Decomposition enriches soil. The area becomes temporarily rich for decomposers and scavengers, then quiet as populations rebuild.
**Duration:** Weeks of in-game time for full recovery.

#### Disease Outbreak
Triggered when: population density is very high, conditions are warm and wet, or the player carries a pathogen between biomes (an invisible consequence of travel).
**Effect:** Mortality spike in affected species. Other species benefit (reduced competition). The player can be affected if they're a compatible host (evolution-dependent).
**Duration:** Variable. Some diseases burn through quickly. Others persist.

### Seasonal Events (if implemented)

#### Spring Emergence
- Mass hatching/birth events across biomes
- Migratory species arrive
- Plant growth surge
- Water levels high from snowmelt
- Maximum biodiversity

#### Summer Drought
- Water levels drop, concentrating creatures at remaining water sources
- Fire risk increases
- Some biomes become impassable (dried marshes)
- Others become accessible (exposed riverbeds)
- Heat stress affects some species

#### Autumn Migration
- Large-scale species movements
- Frantic foraging (pre-winter accumulation)
- Breeding season for some species
- Foliage change (visual transformation of forest biomes)
- Mushroom/fungi fruiting season

#### Winter Dormancy
- Many species hibernate or dormant
- Reduced creature density
- Snow and ice change traversal (frozen waterways become walkable, slopes become slippery)
- Starvation risk increases for active species
- Thermal shelters become critical

### Bloom Events

#### Fungal Bloom
Triggered by: specific humidity/temperature conditions, presence of sufficient organic substrate.
**Effect:** Massive fungal fruiting across a biome. Bioluminescent species create spectacular light displays at night. Spore density increases (potential evolution trigger). Spore network connectivity peaks. Some fungal species are food sources; others are toxic.
**Duration:** 1-3 in-game days.

#### Algal Bloom
Triggered by: nutrient runoff into aquatic systems (following heavy rain in fertilized areas).
**Effect:** Water visibility drops. Oxygen levels in water fluctuate (dangerous for aquatic creatures, including the player if aquatically evolved). Fish kills may occur. Creates food for filter feeders. Water becomes temporarily hazardous.
**Duration:** 2-5 in-game days.

#### Insect Hatch
Triggered by: seasonal timing + weather conditions (warm, calm, often at dusk).
**Effect:** Massive insect emergence. Clouds of insects near water sources. Insectivore species in feeding frenzy. Excellent food source for insect-eating player evolutions. Can impair visibility in dense swarms. Attracts insectivore predators (potential danger).
**Duration:** 1-2 in-game hours (brief, intense).

### Decomposition Events (Large Creature Death)

When a large creature dies (apex predator, megafauna, or any creature above a size threshold), it triggers a multi-stage ecological event:

**Stage 1: Fresh (0-6 in-game hours)**
- Carcass is fresh. Scavengers approach cautiously.
- Largest scavengers feed first (dominance hierarchy).
- Predators may guard the carcass from scavengers (if they killed it).
- Flies and insects begin arriving.
- Smell radius is small.

**Stage 2: Bloated (6-24 in-game hours)**
- Carcass bloats with decomposition gases.
- Smell radius increases dramatically.
- Scavenger activity peaks. Multiple species competing for access.
- Insect colonization intensive.
- The carcass becomes a social hotspot -- observe interspecies interactions.

**Stage 3: Active Decay (1-3 in-game days)**
- Carcass losing mass rapidly.
- Maggots and larvae visible.
- Fewer large scavengers (less meat remaining), more insects and small scavengers.
- Smell radius at maximum.
- Soil beneath carcass becoming nutrient-enriched.

**Stage 4: Advanced Decay (3-7 in-game days)**
- Mostly bones and tough tissue remaining.
- Insect activity declining.
- Plant growth beginning in enriched soil around carcass.
- Bone-eating specialists arrive (if they exist in this biome).
- A micro-ecosystem has established around the remains.

**Stage 5: Remains (7+ in-game days)**
- Skeletal remains only.
- Plants growing through and around bones.
- The temporary micro-biome dissipates.
- A permanent nutrient hotspot remains, resulting in lush plant growth at the site for a long period.
- The bones themselves become sheltering spots for small creatures.

**Player interaction:** Each stage offers different opportunities. Fresh carcasses are food sources (if the player can compete with scavengers). Bloated carcasses attract creatures the player might want to observe. Active decay sites are rich in insect food. Advanced decay sites grow rare plants. Remains become landmarks and shelter.

---

## 5. Player-Ecosystem Interaction

### The Player as Ecosystem Participant

The player creature is subject to the same rules as every other organism:

- **Energy budget:** The player needs food. Not eating reduces energy, reducing movement speed, eventually causing starvation death. Different evolutions have different energy requirements (large adaptations cost more to maintain). Food sources depend on evolution (herbivore, insectivore, carnivore, omnivore paths).

- **Predation risk:** The player can be hunted. Predators don't know the player is special. They assess the player creature as a potential prey item based on size, speed, and behavior. If the player reads as "prey," predators will hunt them. If the player evolves sufficiently threatening features (size, toxicity, armor), predators may avoid them -- but this costs energy and forecloses other evolutionary paths.

- **Territorial status:** The player can be treated as a territorial intruder. Entering a creature's territory may trigger defensive behavior. The player can potentially establish their own territory (through repeated presence and shelter-building), which other creatures learn to avoid.

- **Ecological impact:** Everything the player does affects the ecosystem. Killing a creature removes it from the population. Eating plants reduces vegetation. Disturbing nests causes relocation. Frightening prey drives them to different feeding grounds. Killing a predator cascades through the food web. The player is not exempt from ecological consequence.

### Downstream Effects of Player Actions

#### Killing a Predator
- **Immediate:** One less predator in the biome.
- **Short-term (hours to days):** Prey species in the predator's former territory become bolder, expanding their range.
- **Medium-term (days to weeks):** Prey population increases. Vegetation shows increased grazing pressure. Competing predator species may expand into the vacated territory.
- **Long-term (weeks):** Potential prey population overshoot, followed by vegetation depletion, followed by prey population crash. The biome may take significant time to reach a new equilibrium.

#### Killing Prey Repeatedly in One Area
- **Immediate:** Local prey population reduced.
- **Short-term:** Remaining prey become warier (learned response). Predators in the area lose food source.
- **Medium-term:** Predators may leave the area (migration due to food scarcity) or become more desperate (increased aggression, wider hunting range -- potentially including the player).
- **Long-term:** Vegetation recovers in the absence of herbivore pressure. Biome shifts toward different plant composition.

#### Disturbing a Nest
- **Immediate:** Parent creatures become aggressive (defending offspring). Stress vocalizations alert nearby conspecifics.
- **Short-term:** If the nest is destroyed, the breeding pair/group relocates. Their new nest location may conflict with other territories.
- **Medium-term:** Reduced reproductive success for that group/season. Population impact proportional to the species' reproductive rate.
- **Long-term:** If the player repeatedly disturbs nests in an area, the species may abandon that area entirely.

#### Repeated Presence in an Area (Habituation)
- **Immediate:** Creatures react to the player as a potential threat (flee, hide, defend).
- **Short-term (after several visits):** Creatures begin to habituate if the player is non-threatening. Flee distances decrease. Some individuals show curiosity.
- **Medium-term (after many visits):** Habituation is established for some individuals. They tolerate the player at closer range. New behavioral observations become available (the creature journal fills in details only visible at close range).
- **Long-term:** The player becomes a "known" presence. Some species may actively associate with the player (following for protection, using the player as a kleptoparasitism target, or simply tolerating proximity).

**Important:** Habituation is per-individual, not per-species. A habituated creature's offspring may not be habituated. A creature from a different area won't recognize the player. Moving to a new biome resets the social dynamic.

### Creature Learning

Some creatures learn the player's patterns:

- **Predators that have failed to catch the player** adapt their strategy. If the player always escapes by climbing, the predator may attempt to cut off climbing routes. If the player always flees in a specific direction, the predator may set up an ambush on that route.

- **Prey that has been hunted by the player** becomes warier specifically of the player's approach pattern. If the player always approaches from downwind, the prey begins checking upwind. If the player hunts at dusk, the prey may shelter earlier.

- **Learning decay:** Creature memory fades if the player is absent from an area for extended periods. A predator that learned the player's tricks will partially forget them after the player leaves for several in-game days.

- **Social learning:** Some species transmit learned information socially. If one individual in a group learns that the player is dangerous, the entire group becomes warier. This is based on real behavioral ecology (alarm calling, social learning in corvids/primates).

### Environmental Modification by the Player

The player can modify the environment through evolved abilities:

- **Burrowing:** Creates tunnels that persist. Other small creatures may use player-created tunnels. Tunnels may redirect water flow during rain. Over time, tunnel networks affect soil stability.
- **Nest-building:** Player-built shelters become part of the environment. If abandoned, other creatures may claim them.
- **Pheromone marking:** Player scent markers persist for in-game hours to days. Other creatures react to them (predators may investigate, prey may avoid). Heavy marking in an area creates an olfactory signature that changes creature behavior.
- **Feeding impact:** Consistent harvesting of a food source reduces its availability. Over-harvesting can locally eliminate a plant species, changing the biome composition.

---

## 6. Technical Implementation Notes

### Data Structures

#### Creature Entity (Tier 1)
```
CreatureData:
  species_id: int
  unique_id: int
  position: Vector2
  velocity: Vector2

  # Biological state
  energy: float (0.0 - 1.0)
  health: float (0.0 - 1.0)
  age: float
  size: float

  # Behavioral state
  current_behavior: enum (foraging, hunting, fleeing, resting, traveling,
                          defending_territory, mating, caring_for_young,
                          drinking, socializing)
  behavior_target: reference (food source, prey, shelter, territory center, mate)
  awareness_state: enum (unaware, alert, alarmed, panicked)

  # Memory
  known_threats: array of (position, type, timestamp)
  known_resources: array of (position, type, timestamp)
  player_familiarity: float (0.0 - 1.0, increases with non-threatening exposure)
  learned_player_patterns: array of (pattern_type, confidence)

  # Social
  group_id: int (0 = solitary)
  dominance_rank: int (within group)
  offspring: array of unique_id
  territory_center: Vector2
  territory_radius: float
```

#### Biome State (Tier 2)
```
BiomeData:
  biome_id: int
  biome_type: enum

  # Populations
  species_populations: dict of (species_id -> PopulationData)

  # Environment
  current_weather: WeatherState
  water_level: float
  vegetation_density: float
  nutrient_level: float

  # Events
  active_events: array of EcologicalEvent
  recent_events: array of (event_type, timestamp)

  # Player history
  player_last_visit: timestamp
  player_impact_score: float
  significant_player_actions: array of (action_type, timestamp, location)

PopulationData:
  count: int
  average_health: float
  birth_rate_modifier: float
  death_rate_modifier: float
  migration_pressure: float (positive = emigrating, negative = immigrating)
  notable_individuals: array of CreatureData (tracked even at Tier 2)
```

### Weather State Machine

Weather transitions use a state machine with probabilistic transitions:

```
WeatherStateMachine:
  current_state: WeatherType
  transition_progress: float (0.0 - 1.0, where 1.0 = fully transitioned)
  next_state: WeatherType (determined when transition begins)

  # Every update tick:
  1. If not transitioning:
     - Roll against transition probability table (biome-specific, time-of-day modified)
     - If transition triggered, select next_state from probability table
     - Begin transition (set transition_progress = 0.0)

  2. If transitioning:
     - Increment transition_progress based on transition speed
       (varies by state pair -- clear->storm is slow, rain->heavy_rain is fast)
     - Interpolate visual/audio/gameplay effects between current and next state
     - At transition_progress = 1.0, current_state = next_state, clear transition

  3. Apply current weather effects to:
     - Creature behavior modifiers
     - Player traversal modifiers
     - Visibility range
     - Sound propagation
     - Scent trail persistence
     - Water level change rate
```

### Ecological Update Loop

```
EcologicalUpdate (runs on timer, not every frame):

  # Tier 1: Full simulation (current biome)
  For each creature in active biome:
    update_needs(creature)         # hunger, thirst, energy
    evaluate_threats(creature)     # scan for predators, environmental hazards
    evaluate_opportunities(creature) # scan for food, mates, shelter
    make_decision(creature)        # choose behavior based on needs/threats/opportunities
    execute_behavior(creature)     # act on decision
    update_memory(creature)        # record relevant events
    check_reproduction(creature)   # breed if conditions met
    check_death(creature)          # die if conditions met

  # Tier 2: Simplified simulation (adjacent biomes)
  For each adjacent biome:
    update_populations(biome)      # birth/death statistics
    check_migration_triggers(biome) # population pressure, resource depletion
    execute_migrations(biome)      # move populations between biomes
    update_environment(biome)      # vegetation growth, water levels
    propagate_weather(biome)       # weather state changes

  # Tier 3: Statistical simulation (distant biomes)
  For each distant biome:
    update_population_statistics(biome)  # simple growth/decline equations
    update_carrying_capacity(biome)      # seasonal, long-term changes
```

### Performance Safeguards

- **Creature cap:** Maximum 150 individual creatures in Tier 1 at any time. If biome population would exceed this, lowest-priority creatures (far from player, non-interacting) are managed as sub-groups rather than individuals.
- **Decision throttling:** Creatures in low-activity states (resting, traveling in open terrain) make decisions less frequently. Creatures in high-activity states (hunting, fleeing, interacting with player) make decisions more frequently.
- **Spatial hashing:** Creatures only check for interactions (predation, socializing, mating) with other creatures in nearby spatial hash cells, not with every creature in the biome.
- **Event batching:** Multiple similar events (several creatures fleeing from same threat) are processed as a batch rather than individually.
- **Graceful degradation:** If frame time budget is exceeded, the simulation reduces fidelity smoothly (fewer decision updates, larger spatial hash cells, deferred non-critical behaviors) rather than dropping frames.
