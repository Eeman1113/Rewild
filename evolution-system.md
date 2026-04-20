# Evolution System Design Document

## Table of Contents

1. [Evolution Philosophy](#evolution-philosophy)
2. [How Evolution Triggers](#how-evolution-triggers)
3. [Evolution Tree](#evolution-tree)
   - [Thermal Branch](#thermal-branch)
   - [Aquatic Branch](#aquatic-branch)
   - [Fungal Branch](#fungal-branch)
   - [Arboreal Branch](#arboreal-branch)
   - [Burrowing Branch](#burrowing-branch)
   - [Armored Branch](#armored-branch)
   - [Sensory Branch](#sensory-branch)
4. [Soft-Lock Prevention](#soft-lock-prevention)
5. [Evolution Visual Language](#evolution-visual-language)
6. [Evolution and Narrative](#evolution-and-narrative)

---

## Evolution Philosophy

### Core Principles

**"You Become What You Survive."**

This is the fourth pillar of Rewild, and the evolution system is its mechanical expression. The player's creature does not evolve by collecting experience points, finding upgrade stations, or making selections from a menu screen. It evolves because its body responds to sustained environmental pressure. The creature that spends hours in flooded tunnels begins developing aquatic adaptations. The creature that survives the thermal corridors of the Server District grows heat-dissipation structures. The creature that lives in the canopy develops gripping pads and gliding membranes. The body is a record of the journey. A player who has been everywhere looks like they have been everywhere.

**Permanence. No respec.**

Every evolutionary choice is permanent. There is no mechanic to undo a mutation, revert to a previous form, or reset the evolution tree. This is biological reality: organisms do not un-adapt. A creature that grew gills does not un-grow them because it moved to dry land. The gills are part of the body now. They may become vestigial (reduced in function, partially absorbed) if the player evolves heavily in a non-aquatic direction, but they never disappear. The creature carries the full history of its adaptations.

This permanence creates weight. The player does not spend evolution casually. Each choice closes some futures and opens others. A heavily aquatic creature will struggle in the canopy. A heavily arboreal creature will struggle underground. The player who tries to be everything will be mediocre at everything. The player who commits will excel in their chosen environment and face genuine challenge outside it. This tension between specialization and generalism is the central strategic question of the evolution system.

**Not a skill tree. A biological response.**

The evolution system is not a menu of upgrades arranged in a tree structure. It is a biological simulation of adaptive response to environmental pressure. The distinction matters for design:

- Skill trees present all options simultaneously. The evolution system reveals options only when the environment triggers them.
- Skill trees allow deliberate planning. The evolution system requires experiential discovery — the player must survive in an environment to learn what adaptations it offers.
- Skill trees are transparent. The evolution system is semi-opaque — the player knows something is coming (the body shows pre-mutation stress responses) but not exactly what options will appear.
- Skill trees are pure benefit. The evolution system always involves tradeoffs. Every gain has a cost.

**Adaptation, not upgrade.**

No mutation is universally "better." Every mutation trades performance in one axis for performance in another. Gills allow breathing underwater but reduce lung efficiency in thin air. A heavy carapace provides impact protection but reduces speed and climbing ability. Thermal fins dissipate heat effectively but are vulnerable to cold. The evolution system does not have a power curve — it has a specialization curve. The player does not get "stronger" in a general sense; they get better adapted to specific environments and worse adapted to others.

**The body tells the story.**

The player creature's visual appearance is a complete record of its evolutionary history. Every mutation changes the silhouette, the texture, the color, and the movement animation. A creature that evolved through the Fungal Depths has bioluminescent markings. A creature that evolved through the Canopy has elongated limbs and membranous flaps. A creature that survived the Salt Flats has a matte, armored surface. The creature absorbs the colors and shapes of the biomes that shaped it. By endgame, looking at a player's creature tells the experienced observer exactly where that creature has been and what it survived. The body is the narrative.

---

## How Evolution Triggers

### The Pressure-Adaptation Cycle

Evolution in Rewild follows a three-phase cycle:

**Phase 1: Environmental Pressure**

The player enters a biome with conditions that stress their current body plan. This stress is communicated through gameplay and visual feedback:
- Mechanical stress: reduced stamina recovery in heat, health drain in toxic environments, slower movement in water, etc.
- Visual stress: the creature's animations change — labored breathing in thin air, shivering in cold, sluggish movement in water, squinting in bright light.
- Audio stress: breathing sounds change, movement sounds change, the creature vocalizes discomfort.

The player feels the environmental pressure through the game's systems. They are not told "you are under evolutionary pressure" — they experience it as mechanical difficulty that their current body plan cannot solve.

**Phase 2: Adaptation Threshold**

Sustained exposure to environmental pressure fills an internal "adaptation meter" that is not directly visible to the player. The meter has no UI element — instead, it is communicated through escalating physical changes on the creature:

- **Early stress (0-30%):** Subtle animation changes. Labored breathing. Slight color shifts on the creature's skin toward the biome's palette.
- **Mid stress (30-60%):** Visible physical distress. The creature's skin texture begins to change in the affected areas (roughening in heat, slicking in water, dulling in toxic environments). Movement animations show compensation behaviors.
- **Pre-mutation (60-90%):** Dramatic physical indicators. The affected body areas show clear pre-adaptation features: bumps that suggest future carapace, web-like stretching between digits that presages webbed feet, darkened patches that hint at bioluminescent capability. The creature is visibly changing.
- **Threshold (100%):** Evolution triggers.

The rate at which the meter fills depends on:
- Intensity of environmental pressure (deeper in the biome = faster)
- Duration of continuous exposure (staying in the biome fills it faster than visiting briefly)
- Active engagement with environmental hazards (surviving a heat vent pushes the thermal meter more than merely being in a warm area)
- How many mutations the player already has on the relevant branch (later mutations require more pressure — the body has already adapted somewhat, so further change requires stronger stimulus)

**Phase 3: The Evolution Moment**

When the adaptation threshold is reached, the evolution event triggers. This is one of the most important moments in the game, both mechanically and narratively.

**The Moment:**

1. The world does not pause. The world does not acknowledge what is happening. (Pillar: "The World Doesn't Care About You.")
2. The creature's movement slows to a halt. The player loses control input. The creature's body seizes, curls, or stretches depending on the mutation type.
3. The creature's body visibly transforms over 3-5 seconds. This is not instantaneous — the player watches their creature change shape, grow new features, shift color.
4. During the transformation, the world continues. Creatures move past. Weather continues. Anything that was pursuing the player continues pursuing. The transformation does not grant invulnerability. Evolving in the wrong place at the wrong time can be fatal.
5. At the end of the physical transformation, a brief selection interface appears: the player sees 2-3 mutation options available for the current branch, presented as visual previews on the creature's body rather than text descriptions. The player selects one. The selection interface lasts a fixed time (8-10 seconds) after which the game auto-selects based on the creature's current pressures if the player has not chosen.
6. The creature completes the transformation with the selected mutation. Control returns immediately. The world has not paused, so the player must immediately deal with whatever is happening around them.

**The auto-selection fallback** (if the player does not choose within the time window) is deterministic: the game selects the mutation that best addresses the environmental pressure the creature is currently experiencing. This is not a punishment — it is the game's simulation of natural selection. If the player does not exercise agency, the environment exercises it for them. This mirrors the game's themes: the organisms that succeed are the ones that respond to pressure, not the ones that dither.

### Evolution Points and Branching

The player does not accumulate "evolution points" or "mutation currency." The adaptation meter fills per-branch based on environmental exposure. The player's total evolutionary budget is limited by a biological constraint: the creature's genome has a finite capacity for mutation.

**Mutation Slots:** The creature has a maximum of 12 mutation slots. Each mutation occupies one slot. Mutations within a branch can be upgraded (replacing the slot's content with a more advanced version), but this still requires a new threshold event. Reaching 12 slots means the creature is fully evolved — further mutation requires sacrificing an existing mutation (which is narratively framed as the body reclaiming resources, and the lost mutation becomes vestigial — still partially visible but non-functional).

**Branch Depth:** Each branch has 4 tiers of mutation. Tier 1 mutations are broad adaptations available with minimal pressure. Tier 4 mutations are extreme specializations requiring sustained, intense environmental exposure. A player can have mutations from multiple branches (a generalist build with Tier 1-2 mutations from several branches) or deep mutations from one or two branches (a specialist build with Tier 3-4 mutations concentrated in a few branches).

---

## Evolution Tree

### Thermal Branch

**Environmental Trigger:** Prolonged exposure to extreme heat. Server District, volcanic vents (deep underground), geothermal zones, and fire-exposed areas.

**Branch Theme:** The body learns to manage heat — first by surviving it, then by leveraging it.

---

#### Tier 1: Heat Tolerance

**Mechanical Effect:**
- Reduces heat damage by 50%.
- Stamina recovery in hot environments normalizes (removes the heat penalty).
- Player can traverse moderate-heat zones (Server District exterior, surface thermal vents) without damage.

**Player Controller Hooks:**
- `heat_damage_modifier: 0.5`
- `stamina_recovery_modifier` in hot environments returns to baseline.
- Enables traversal through thermal barrier zones rated "moderate."

**Visual Change:**
- Skin darkens in patches, developing a matte, heat-absorbing texture.
- Subtle color shift toward warm tones (the creature begins absorbing the Server District's palette).
- Silhouette unchanged.

**Environments Opened:**
- Server District interior corridors (moderate heat zones).
- Surface geothermal areas in the Grid Forest and Industrial Wetlands.

**Tradeoff:**
- Cold environments cause 25% more stamina drain. The body has tuned itself for heat; cold is now more stressful.

**Silhouette Impact:** Minimal. Texture and color change only.

---

#### Tier 2: Thermal Vision

**Mechanical Effect:**
- Activatable sense mode that reveals heat signatures through walls and obstacles within a 15-meter radius.
- Living creatures, active machinery, and thermal vents are visible as warm-colored auras.
- The sense mode replaces normal vision while active (the world is rendered in thermal palette — warm = bright, cool = dark).
- Duration: 10 seconds. Cooldown: 20 seconds.

**Player Controller Hooks:**
- New input: `activate_thermal_vision` (toggle on/off within duration).
- Vision shader swap to thermal palette.
- Detection layer reveals heat sources through occluding geometry within radius.
- Cooldown timer prevents continuous use.

**Visual Change:**
- Eyes develop a visible reflective layer (tapetum-like) that glows faintly orange in dark environments.
- The eye change is visible in the creature's idle animation.
- Silhouette change: eyes are slightly larger, more prominent.

**Environments Opened:**
- Navigation in total darkness using thermal vision (Fungal Depths, Deep Archive) — living creatures and geothermal warmth in walls serve as landmarks.
- Detection of hidden creatures (Moss Elementals, Checkout Mimics) through their body heat before they are visually identifiable.

**Tradeoff:**
- Normal visual acuity in bright environments is slightly reduced (the reflective eye layer causes glare). Bright environments (Salt Flats, Canopy surface) become marginally harder to navigate visually.

**Silhouette Impact:** Moderate. Enlarged, prominent eyes change the head silhouette.

---

#### Tier 3: Heat Dissipation Fins

**Mechanical Effect:**
- Passive heat management system. The creature can now operate in extreme-heat environments (Server District deep interior, active thermal vents) indefinitely without heat damage.
- Heat buildup from any source dissipates 3x faster.
- New mechanic: the fins can be "charged" by heat exposure and then discharged as a radial heat pulse that deters temperature-sensitive creatures within a 5-meter radius. Non-damaging, but interrupts creature attacks and creates a brief window for repositioning.

**Player Controller Hooks:**
- `heat_damage_modifier: 0.0` (full immunity to environmental heat at normal gameplay levels).
- `heat_dissipation_rate: 3.0` (passive).
- New ability: `heat_pulse` — requires heat charge (accumulated by being in hot environments), 3-meter radius push effect on creatures, 30-second cooldown after discharge.

**Visual Change:**
- Fin-like protrusions along the spine, shoulders, and tail (if evolved). The fins are flat, vascularized structures with a subtle color gradient — dark at the base (body heat), bright at the tips (dissipated heat). They flutter and fan in hot environments.
- The creature's overall color warms significantly. Reds, oranges, and deep ambers dominate the affected body areas.
- The fins glow faintly when heat-charged (pre-pulse visual indicator).

**Environments Opened:**
- Full access to all Server District areas including the deepest, hottest server rooms.
- Survival in fire-exposure events (forest fires in the Grid Forest, spontaneous methane combustion in the Fungal Depths).
- Heat pulse enables escape from otherwise overwhelming close-range encounters.

**Tradeoff:**
- The fins increase the creature's visual profile (larger silhouette, glowing elements). Stealth is harder. Creatures that detect by visual silhouette find the player more easily.
- Cold environments now cause active damage, not just stamina drain. The fins lose heat rapidly in cold, causing hypothermic stress.
- Swimming speed reduced by 15% (fin drag in water).

**Silhouette Impact:** Major. The spinal fins dramatically change the creature's profile from all angles. The creature is visibly a thermal specialist.

---

#### Tier 4: Infernal Metabolism

**Mechanical Effect:**
- The creature's metabolism is heat-fueled. Proximity to heat sources (thermal vents, fire, the Server District's ambient heat, even warm-blooded prey) provides a regeneration effect: health recovery of 2% per second when near significant heat.
- The heat pulse evolves into a directed heat beam: a short-range (3 meter) cone attack that deals significant damage. This is the only directly damaging attack evolution in the Thermal Branch. Cooldown: 15 seconds.
- Heat immunity is absolute. Lava, direct fire, and extreme thermal events cannot damage the creature.

**Player Controller Hooks:**
- `heat_proximity_regen: 0.02` per second within 5 meters of a heat source rated "significant."
- Replaces `heat_pulse` with `heat_beam`: cone attack, 3m range, 45-degree spread, damage scaled to creature's current heat charge, 15-second cooldown.
- `heat_damage_modifier: 0.0` for all heat sources including extreme events.

**Visual Change:**
- The creature's body develops visible vascular networks beneath semi-transparent skin in the fin and torso areas. These networks glow with metabolic heat — the creature is visibly warm-blooded in an extreme way, its circulatory system a visible map of light beneath its surface.
- The overall color shifts to deep volcanic tones: dark reds, black, with veins of bright orange and yellow.
- The fins are fully developed and constantly active — fanning, adjusting, and radiating heat.
- In cold environments, the creature visibly steams as its body heat meets cold air.

**Environments Opened:**
- Access to the most extreme thermal zones in the game — the deepest Server District chambers where servers are in active meltdown, volcanic features deep underground.
- The heat beam provides a combat option that transforms difficult predator encounters (creatures immune to other forms of deterrence may be vulnerable to extreme heat).

**Tradeoff:**
- Cold environments are now actively lethal. The creature's heat-dependent metabolism fails in cold conditions, causing rapid health drain. The upper floors of the Vertical Gardens in winter, the wind-exposed Canopy, and cold-water submersion are all dangerous-to-fatal.
- The creature's heat signature makes it the brightest thermal target in any environment. Thermal-detecting predators (Heat Vipers, Mycelial Hunters using thermal signatures) acquire the player from much greater range.
- Aquatic capability is severely compromised: water saps heat, and the creature cannot sustain submersion for long.

**Silhouette Impact:** Dramatic. The creature is unmistakably a thermal organism. The glowing vascular network and prominent fin array create a silhouette unlike any other build path. Another player seeing this creature knows exactly where it has been.

---

### Aquatic Branch

**Environmental Trigger:** Prolonged water exposure. Flooded Transit Corridors, Sunken Campus, Reservoir, Industrial Wetlands, and any submerged environment.

**Branch Theme:** The body returns to water — a reversal of the evolutionary step that brought animal life to land.

---

#### Tier 1: Gill-Lungs

**Mechanical Effect:**
- The creature can breathe underwater indefinitely. Submerged movement is no longer time-limited by breath.
- Surface breathing is unaffected. The gill-lungs are a hybrid system.
- Underwater movement speed increases by 20% (the body relaxes in water rather than struggling).

**Player Controller Hooks:**
- `breath_timer: disabled` when submerged (infinite underwater time).
- `water_movement_speed_modifier: 1.2`
- Transition animation between surface and water breathing modes.

**Visual Change:**
- Gill slits appear on the neck/throat area. They are subtle at rest (closed) but open and pulse when underwater.
- Skin in the gill area develops a slightly translucent, moist quality.
- Silhouette change: minimal from front, visible from side (gill flap profile).

**Environments Opened:**
- Full exploration of flooded Transit Corridors underground sections.
- Extended underwater exploration in the Sunken Campus.
- Access to submerged passages that connect biomes (many underground connections require water traversal).

**Tradeoff:**
- The gill tissue is sensitive. Airborne irritants (spore clouds, chemical fog, dust) cause 25% more respiratory damage. The gills cannot close fully in air and admit particles that pure lungs would filter.
- Dry environments (Salt Flats, upper Vertical Gardens floors) cause the gill tissue to dehydrate, creating a slow health drain when far from water for extended periods.

**Silhouette Impact:** Minimal. Neck area changes visible on close inspection.

---

#### Tier 2: Webbed Limbs

**Mechanical Effect:**
- Underwater movement speed increases to 50% above baseline (total, replacing Tier 1 bonus).
- New swimming animation with fully articulated limb strokes.
- Underwater maneuverability greatly improved: tighter turns, faster acceleration, vertical swimming.
- Land movement speed reduced by 10% (webbed feet are less efficient on solid ground).

**Player Controller Hooks:**
- `water_movement_speed_modifier: 1.5`
- `water_turn_rate_modifier: 1.8`
- `water_acceleration_modifier: 1.5`
- `ground_movement_speed_modifier: 0.9`
- New swim animation set replacing the default underwater movement animations.

**Visual Change:**
- Visible webbing between fingers and toes. The webbing is translucent, vascularized membrane.
- Limb proportions shift slightly: hands and feet are broader, flatter.
- The creature's movement on land has a slightly splayed, ungainly quality that emphasizes the aquatic specialization.
- Color shift toward blues and greens, absorbing the palette of aquatic environments.

**Environments Opened:**
- Underwater exploration becomes viable as a primary movement mode. The player can move through water faster and more precisely than most aquatic creatures, enabling aquatic chase and evasion.
- Strong underwater currents (Transit Corridor flash floods, Reservoir intake zones) become navigable rather than lethal.

**Tradeoff:**
- Ground speed reduction affects all terrestrial movement. The creature is noticeably slower on land.
- Climbing ability reduced by 20% — webbed hands grip less effectively on vertical surfaces.
- The webbed silhouette is visible to underwater predators from greater distance (larger visual profile in water).

**Silhouette Impact:** Moderate. Broader hands and feet, visible webbing in spread-limb poses (jumping, climbing, swimming).

---

#### Tier 3: Pressure Resistance

**Mechanical Effect:**
- The creature can descend to the deepest submerged areas without pressure damage. Deep-water zones (Reservoir deep tanks, flooded infrastructure deep sections) are now accessible.
- Crush resistance translates to general impact resistance: fall damage reduced by 30%.
- The creature can hold position in strong currents that would push other organisms.

**Player Controller Hooks:**
- `depth_pressure_damage: 0.0` (full immunity to pressure effects).
- `fall_damage_modifier: 0.7`
- `current_resistance_modifier: 2.0` (force required to push the creature is doubled).

**Visual Change:**
- The creature's body becomes denser, more compact. Musculature visible beneath tightened skin.
- Ribcage area develops visible reinforcement — the structure beneath the skin suggests additional skeletal support.
- Overall color deepens toward the dark blues and blacks of deep water.
- The creature appears heavier, more substantial. Movement animations reflect the increased mass.

**Environments Opened:**
- Reservoir deep sedimentation tanks (the deepest aquatic environment in the game).
- Deep flooded sections of the Fungal Depths.
- The fall damage reduction opens some vertical shortcuts that would otherwise be lethal.

**Tradeoff:**
- Increased body density reduces buoyancy. The creature sinks in water rather than floating — surface swimming requires active effort, and resting on the water surface is no longer possible.
- Jump height reduced by 15% (heavier body).
- Sprint speed on land reduced by an additional 10% (combined with Tier 2: total 20% land speed reduction).
- The compact, heavy body plan conflicts with arboreal evolution. Arboreal Branch mutations beyond Tier 1 become significantly harder to trigger (2x adaptation pressure required) if Pressure Resistance is taken.

**Silhouette Impact:** Moderate. The creature is visibly stockier, more compact. The overall outline is rounder, denser.

---

#### Tier 4: Echolocation

**Mechanical Effect:**
- Activatable sense mode that emits a sonar pulse mapping all solid surfaces, creatures, and objects within a 25-meter radius. The pulse returns a wireframe representation of the environment that persists for 6 seconds.
- Works in total darkness, turbid water, and spore-clouded air. This is the most reliable navigation sense in the game — it works in every environment.
- Reveals hidden creatures (submerged, burrowed, or camouflaged) as solid returns distinct from the environment.
- Duration: 6-second persistence per pulse. Cooldown: 8 seconds (allowing near-continuous use).

**Player Controller Hooks:**
- New input: `echolocation_pulse` (activatable).
- Rendering mode: wireframe overlay on standard vision, showing detected surfaces and entities within radius. Fades over 6 seconds.
- Detection penetrates concealment (water, burrows, camouflage) but not solid walls thicker than 1 meter.
- Cooldown: 8 seconds.

**Visual Change:**
- The creature's skull structure visibly changes: the jaw and forehead develop resonant structures that amplify and direct the sonar pulse. The head is slightly larger and more elongated.
- Ear structures are dramatically enlarged and repositioned — the creature's hearing apparatus is the most visible feature on its head.
- When echolocation is active, the pulse is visible as a ripple emanating from the creature's head — a visual representation of the sound wave.
- Color shift: the head and sensory structures take on the deepest blues and blacks, contrasting with the body.

**Environments Opened:**
- Effective navigation in all dark environments (Fungal Depths, Deep Archive) without relying on bioluminescence or external light.
- Detection of ambush predators in any environment (Checkout Mimics, Glow Lures, Moss Elementals) before they can strike.
- Underwater navigation without visual reliance — turbid, dark, or algal-bloom conditions become fully navigable.

**Tradeoff:**
- Sound sensitivity is greatly increased. Loud environments (Rush Hour Swarms, Migration Corridor herds, electrical storms) cause sensory overload that temporarily disables echolocation and reduces normal hearing.
- The echolocation pulse is audible to sound-sensitive creatures. Using echolocation near Lecture Hall Bats, Tunnel Eels, or other sonar-sensitive organisms alerts them to the player's position.
- The enlarged head structure increases the creature's profile, making it marginally more visible to visual predators.
- Persistent use creates a faint hum that prevents stealth in quiet environments.

**Silhouette Impact:** Major. The head restructuring is dramatic and visible from all angles. The creature's profile is immediately identifiable as an echolocating organism.

---

### Fungal Branch

**Environmental Trigger:** Prolonged exposure to fungal environments, toxic zones, and chemical hazards. Fungal Depths, Hospital Quarter, Industrial Wetlands, and any area with dense spore activity.

**Branch Theme:** The body learns to coexist with the world's dominant kingdom — first by resisting it, then by integrating with it.

---

#### Tier 1: Spore Membrane

**Mechanical Effect:**
- Toxin and spore damage reduced by 50%.
- The creature can breathe normally in moderate spore clouds without disorientation or health effects.
- Minor poison resistance applies to all biological toxin sources (venomous creature attacks, toxic water).

**Player Controller Hooks:**
- `toxin_damage_modifier: 0.5`
- `spore_effect_modifier: 0.5` (reduced duration and intensity of spore-induced effects).
- `poison_resistance: 0.3` (30% reduction in biological poison damage from creature attacks).

**Visual Change:**
- Skin develops a faint, matte coating — visible as a slight textural difference, like a dusting of fine powder.
- In spore-heavy environments, the coating becomes more visible as it accumulates and neutralizes spores.
- Slight green-gray color shift in the affected areas (usually torso and face first).

**Environments Opened:**
- Hospital Quarter spore zones become navigable without immediate health consequences.
- Fungal Depths entry areas (shallow spore concentrations) become survivable for extended exploration.
- Industrial Wetlands chemical fog causes reduced damage.

**Tradeoff:**
- The membrane slightly reduces tactile sensitivity. Touch-based environmental interaction (detecting vibrations through surfaces, feeling temperature changes through skin) is dulled by 20%.

**Silhouette Impact:** Minimal. Texture change only.

---

#### Tier 2: Toxin Resistance

**Mechanical Effect:**
- Full immunity to environmental toxins at moderate concentration. The creature can operate in chemical fog, toxic water, and spore storms without health impact.
- Venomous creature attacks deal 60% reduced damage.
- The creature can consume toxic organic material as food (fungal fruiting bodies, chemical-adapted organisms) without health penalty.

**Player Controller Hooks:**
- `toxin_damage_modifier: 0.0` for moderate-concentration environmental toxins.
- `poison_resistance: 0.6`
- New food sources unlocked: toxic organic materials are now metabolizable.

**Visual Change:**
- The membrane is now visibly integrated into the skin. The creature's skin has a waxy, slightly iridescent quality — the toxin-resistance chemistry beneath the surface creates subtle color play.
- Colors shift further into fungal territory: greens, purples, and grays.
- The creature's eyes develop a third eyelid (nictitating membrane) that closes over the eyes in toxic environments — a visible indicator of the toxin defense engaging.

**Environments Opened:**
- Full access to the Hospital Quarter, including the pharmaceutical basement zones.
- Industrial Wetlands chemical water becomes traversable (combined with aquatic adaptation, the player can swim in chemical water).
- Fungal Depths mid-depth zones become survivable.

**Tradeoff:**
- The creature's scent changes. Chemical processing produces metabolic byproducts that alter the creature's chemical signature. Creatures that hunt by chemical detection (Mycelial Hunters, chemical-sensing predators) detect the player from 50% greater distance.
- The waxy skin coating reduces the effectiveness of thermal regulation. Heat and cold extremes are slightly more impactful (+15% thermal damage from all sources).

**Silhouette Impact:** Minimal. Skin quality change and nictitating membrane visible but do not change outline.

---

#### Tier 3: Bioluminescence

**Mechanical Effect:**
- The creature can produce controllable bioluminescence from patches on its body. Functions as a built-in light source in dark environments with adjustable intensity (dim ambient to bright focused).
- New mechanic: the light can be used as a lure. Activating bright, pulsing bioluminescence in fungal environments attracts phototropic creatures to the player's position — useful for distraction, drawing creatures away from a target, or baiting predators into traps.
- New mechanic: bioluminescence color can be shifted to mimic local fungal glow patterns, reducing detection by fungal-network-aware creatures. The creature "speaks the language" of bioluminescence.

**Player Controller Hooks:**
- New input: `bioluminescence_toggle` (on/off, with intensity slider: dim/medium/bright).
- `bioluminescence_lure`: bright pulsing mode attracts phototropic creatures within 20-meter radius.
- `bioluminescence_mimic`: color-match mode reduces detection by fungal-network creatures by 60% while in fungal environments.
- Light source attached to player model, illuminates environment based on intensity setting.

**Visual Change:**
- Luminescent patches appear on the creature's body — concentrated on the face, shoulders, flanks, and tail/rear. The patches follow the creature's existing skin patterns, making the glow feel organic rather than overlaid.
- In darkness, the creature is its own light source. The visual effect is striking: a softly glowing organism in pitch blackness.
- The bioluminescence color adapts to the local fungal palette — green in the Fungal Depths, blue in the Server District, amber in the Hospital Quarter.
- When mimicry mode is active, the glow pattern shifts to match surrounding fungal rhythms (pulsing in sync with the local network).

**Environments Opened:**
- Dark environments (Fungal Depths, Deep Archive) become self-illuminated.
- Fungal Depths navigation is transformed: the creature can move through the network with reduced detection if mimicry mode is maintained.
- The lure mechanic opens puzzle-like encounters where the player draws creatures to specific locations.

**Tradeoff:**
- The bioluminescence cannot be fully suppressed. Even when "off," the creature retains a faint residual glow. In dark environments where other organisms rely on visual detection, this makes the creature marginally more visible than a non-luminescent creature.
- In bright environments, the luminescent patches absorb light differently from normal skin, creating visual contrast that makes the creature easier to spot against sun-lit backgrounds.
- Energy cost: maintaining bioluminescence drains stamina slowly. Bright mode drains faster. The creature cannot sprint while maintaining bright bioluminescence.

**Silhouette Impact:** Moderate in dark environments (the creature is a glowing shape), minimal in lit environments (patches visible but do not change outline).

---

#### Tier 4: Mycelial Network Sensing

**Mechanical Effect:**
- The creature can interface with the mycorrhizal fungal network that underlies most of the game world. While in contact with the ground in areas where the network extends (most biomes except the Salt Flats and upper Canopy), the creature can "read" the network:
  - Detect the location and movement of all creatures within 50 meters that are also in contact with the ground.
  - Sense the general "health" of the local ecosystem (nutrient flow, stress responses, population density).
  - Detect the direction and distance of specific biome transitions.
- In the Fungal Depths specifically, the creature can communicate with the network: sending chemical signals that alter the fungal network's behavior in a small area. This can suppress local spore production, redirect Mycelial Hunters, or cause the network to "calm" and stop reporting the creature's presence.

**Player Controller Hooks:**
- New passive: `network_sense` — when grounded, displays creature positions as subtle indicators at the edge of the screen (direction and rough distance). Updates in real time.
- New passive: `ecosystem_read` — provides ambient UI indication of local biological activity level.
- New active (Fungal Depths only): `network_communicate` — sends a chemical signal through the network. Player selects from contextual options: "suppress spores" (clears spore cloud in 10m radius), "redirect hunter" (changes target of nearest Mycelial Hunter to a decoy point), "calm network" (stops detection in 15m radius for 30 seconds). Cooldown: 60 seconds.

**Visual Change:**
- Root-like structures develop on the creature's underside — feet, belly, lower limbs. These are mycelial interface organs that physically connect the creature to the fungal network when in contact with the ground.
- The root-structures pulse with bioluminescence when actively reading the network — visible tendrils of light reaching from the creature's feet into the ground.
- The creature's overall form takes on an increasingly fungal quality: slightly asymmetric growths, organic textures that blur the line between animal and fungus, colors that shift toward the deep purples and phosphorescent greens of the Depths.
- At this evolution level, the creature is no longer entirely "animal." It has become a hybrid organism, part of the network it now reads.

**Environments Opened:**
- The Fungal Depths become navigable at the deepest levels. The creature can suppress the network's detection, redirect its most dangerous defenders, and read the environment like a map.
- Creature detection through the network works in nearly all biomes, providing a global awareness that no other sense can match.
- The ability to read ecosystem health opens hidden content: distressed ecosystem readings can guide the player to hidden biome features, narrative fragments, or evolution triggers they would otherwise miss.

**Tradeoff:**
- The mycelial interface organs are vulnerable. Walking on hard, sterile surfaces (concrete, metal, salt crust) damages them, causing a slow health drain when the player is in environments without organic ground cover. The Deep Archive and Salt Flats become more painful.
- The network connection is bidirectional. When the creature reads the network, the network reads the creature. In the Fungal Depths, The Undershepherd can locate the player instantly through the network if the player is actively using network sense.
- The fungal integration affects the creature's biology at a deep level. Health regeneration in non-fungal environments is reduced by 20% (the body now partially depends on mycelial nutrient exchange).
- The creature's increasingly fungal appearance may trigger aggression from anti-fungal organisms (creatures in clean environments that attack fungal intrusion).

**Silhouette Impact:** Major. The root structures, asymmetric growths, and fungal textures create a creature that is visibly part of two kingdoms. The silhouette is organic, irregular, and unmistakably fungal-integrated.

---

### Arboreal Branch

**Environmental Trigger:** Prolonged time in elevated, three-dimensional environments. The Canopy, Vertical Gardens, Grid Forest upper levels, and Root Cathedral canopy zones.

**Branch Theme:** The body learns to live in the air — to climb, to grip, to glide, to fall without dying.

---

#### Tier 1: Gecko Pads

**Mechanical Effect:**
- The creature can climb vertical surfaces that have texture (rough stone, bark, brick, concrete). Smooth surfaces (glass, wet metal) still cannot be climbed.
- Climbing speed: 60% of ground movement speed.
- Stamina drain while climbing is moderate (climbing is effortful but sustainable for extended periods).

**Player Controller Hooks:**
- Climbing state enabled on surfaces tagged "climbable_rough."
- `climbing_speed: 0.6 * base_movement_speed`
- `climbing_stamina_drain: moderate` (adjustable based on surface type and angle — overhangs drain more).

**Visual Change:**
- Hands and feet develop visible pad structures — broad, flat, with fine lamellae visible on close inspection.
- Digits become slightly more spread, giving hands and feet a wider, more grasping appearance.
- The pads have a different texture from the rest of the skin: smoother, slightly lighter in color.

**Environments Opened:**
- Vertical Gardens floors become accessible via external climbing (bypassing interior hazards).
- Grid Forest transmission towers can be climbed without vine-routes.
- Root Cathedral tree trunks become climbable surfaces.
- General verticality across all biomes — any rough vertical surface becomes a traversal option.

**Tradeoff:**
- The broadened hands and feet are less effective for burrowing. Burrowing Branch mutations beyond Tier 1 require 50% more adaptation pressure if Gecko Pads are taken.
- Running speed on flat ground reduced by 5% (the pad structures affect foot-strike efficiency on hard flat surfaces).

**Silhouette Impact:** Minimal. Broader hands and feet visible in spread-limb poses.

---

#### Tier 2: Gliding Membranes

**Mechanical Effect:**
- The creature develops skin flaps between the forelimbs and body that enable controlled gliding.
- From any height advantage, the player can enter a glide state: forward movement at 150% base speed, gradual descent at a controllable glide ratio (approximately 3:1 horizontal to vertical, adjustable by player input).
- Gliding is wind-affected: headwind reduces speed, tailwind increases it, crosswind pushes laterally.
- Gliding does not provide lift — the creature always descends. This is gliding, not flight.

**Player Controller Hooks:**
- New input: `glide_toggle` (activatable when airborne with sufficient height).
- Glide state: `forward_speed: 1.5 * base`, `descent_rate: controllable within range`, `wind_effect: applied`.
- Player can steer during glide (turn rate limited — wide, sweeping curves, not sharp turns).
- Glide ends on surface contact or stamina depletion (gliding drains stamina slowly).

**Visual Change:**
- Membrane flaps visible along the creature's sides, from forelimb to hindlimb. When not gliding, they fold against the body. When gliding, they spread to full extension, creating a dramatic visual transformation.
- The membranes are translucent, showing vascularization. They glow faintly with bioluminescence if the Fungal Branch is also developing.
- Overall silhouette change: the creature becomes wider in profile when membranes are deployed.

**Environments Opened:**
- The Canopy becomes traversable. Gaps between trees can be crossed by gliding.
- Inter-building traversal in the Vertical Gardens (sky-bridge alternatives).
- Rapid descent from any elevated position without fall damage (gliding dissipates falling energy).
- Long-distance travel between elevated biomes (Grid Forest towers to Vertical Gardens rooftops, for example).

**Tradeoff:**
- The membrane structures increase the creature's wind-exposed surface area. Strong wind affects the creature more — in the Salt Flats, on exposed Vertical Gardens floors, and during storms, the player is pushed more forcefully.
- Swimming efficiency reduced by 10% (membrane drag in water).
- The deployed membrane silhouette is very large and visible. Aerial predators (Summit Eagles, Canopy Wraiths, Smokestack Raptors) spot the creature more easily during glide.

**Silhouette Impact:** Major during glide (the creature doubles in apparent width). Moderate at rest (folded membranes visible as flaps along the body).

---

#### Tier 3: Prehensile Tail

**Mechanical Effect:**
- The creature develops a strong, flexible tail capable of gripping branches and surfaces.
- New mechanic: tail-hang. The creature can suspend from suitable anchor points (branches, pipes, rails, structural beams) using only the tail, freeing all four limbs for other actions.
- New mechanic: tail-swing. From a tail-hang, the player can build momentum and release to launch in a chosen direction. This is the primary rapid-traversal mechanic in three-dimensional environments.
- The tail provides balance assistance: narrow surfaces (beams, power lines, vine bridges) can be traversed at full speed instead of reduced balance-walking speed.

**Player Controller Hooks:**
- New limb: `tail` with grip capability on surfaces tagged "grippable" (branches, pipes, rails, beams, cables).
- `tail_hang` state: suspends creature, frees other limbs, camera adjusts to inverted/hanging perspective.
- `tail_swing`: builds momentum while tail-hanging, release launches creature in aim direction. Distance proportional to momentum built.
- `narrow_surface_balance_modifier: 1.0` (full speed on narrow surfaces, up from default 0.5).

**Visual Change:**
- A muscular, flexible tail grows from the base of the spine. It is as long as the creature's body and visibly prehensile — the tip curls and grips in idle animations.
- The tail is the most dynamically animated part of the creature: it constantly adjusts for balance, reaches toward potential grip points, and curls protectively when the creature is defensive.
- Tail musculature suggests significant strength — the tail can support the creature's full body weight.

**Environments Opened:**
- The Canopy becomes a home environment. Tail-swinging through the branch network is the fastest traversal method in the canopy.
- The Vertical Gardens interior becomes vastly more navigable — tail-hang and swing work on pipes, exposed structural beams, and collapsed fixtures.
- The Grid Forest power-line network becomes a full highway — tail-hanging from vine-covered cables enables rapid transit.

**Tradeoff:**
- The tail adds to the creature's total body length, making it harder to navigate tight spaces. Burrowing through narrow passages becomes more difficult (or requires curling the tail, which is slower).
- The tail is a vulnerable appendage. Attacks that hit the tail cause disproportionate pain response (brief stun/flinch animation). Predators may learn to target it.
- Ground speed slightly reduced — the tail's weight affects running gait. -5% additional ground speed penalty (stacking with other modifiers).

**Silhouette Impact:** Major. The tail is long and constantly in motion. The creature's silhouette is immediately identifiable as arboreal.

---

#### Tier 4: Lightweight Bones

**Mechanical Effect:**
- The creature's skeletal system restructures for minimum weight: hollow bones, reduced density, optimized architecture. Total body mass decreases by 40%.
- Glide ratio improves to 5:1 (from 3:1). The creature can cover enormous horizontal distances from even modest height advantages.
- Jump height increases by 50%. Combined with gliding, the creature can reach positions that seem impossible.
- Fall damage is further reduced (50% of baseline — stacking with any other fall reduction).
- Movement speed on all surfaces increases by 15% (lighter body, same muscle mass).

**Player Controller Hooks:**
- `body_mass_modifier: 0.6`
- `glide_ratio: 5.0` (horizontal distance per unit of vertical descent)
- `jump_height_modifier: 1.5`
- `fall_damage_modifier: 0.5`
- `movement_speed_modifier: 1.15` (applied globally to all movement states)

**Visual Change:**
- The creature becomes visibly leaner. Bone structure is visible through thinner skin at joints and along the spine. The creature looks fast even when still — the body plan communicates lightness.
- Limb proportions shift: longer relative to body, thinner, with visible joint articulation.
- The overall silhouette becomes angular and aerodynamic. The creature in motion is a streaming shape.
- Color tends toward the pale greens and sky blues of the upper canopy — light, airy, high.

**Environments Opened:**
- The Canopy at its most extreme becomes accessible: the highest, most wind-exposed, most isolated branch-tips where only the lightest creatures can land.
- Traversal between distant biome sections becomes possible via extended glide from high points. The creature can glide from a Vertical Gardens rooftop to a Grid Forest tower network in a single flight.
- Areas gated behind "minimum weight" requirements (fragile surfaces, thin ice, unstable structures) become passable.

**Tradeoff:**
- Hollow bones are fragile. Impact damage from creature attacks is 40% more effective. The creature can be hurt more easily by direct strikes.
- Knockback from all sources is dramatically increased. The creature is light enough that wind, water currents, creature attacks, and environmental forces push it much farther. Being hit near an edge means going over the edge.
- Weight-dependent mechanics (forcing open sealed doors, breaking through barriers, holding position in currents) become much harder or impossible.
- Aquatic pressure resistance is severely compromised — hollow bones collapse at depth. Deep-water zones become lethal again even with Aquatic adaptations.

**Silhouette Impact:** Major. The creature is visibly different from its earlier forms: lighter, thinner, more angular. The body plan shouts "sky creature."

---

### Burrowing Branch

**Environmental Trigger:** Prolonged time underground in tight spaces. Root Cathedral root labyrinth, Fungal Depths tunnels, rubble-filled basements, collapsed structures.

**Branch Theme:** The body learns to move through the earth — to dig, to sense without seeing, to compress and expand.

---

#### Tier 1: Hardened Claws

**Mechanical Effect:**
- The creature's claws become dense and powerful, capable of digging through soil, soft rubble, and decayed organic material.
- New mechanic: `dig` — the player can excavate through diggable surfaces, creating temporary passages through soft barriers.
- Melee attack damage increased by 25% (claws are natural weapons).

**Player Controller Hooks:**
- New input: `dig` on surfaces tagged "diggable" (soil, soft rubble, decayed wood, loose debris).
- Dig speed: slow (3-4 seconds per body-length of tunnel in soil, longer in rubble).
- `melee_damage_modifier: 1.25`

**Visual Change:**
- Claws on all four limbs become prominent, dark, and visibly dense. They curve slightly for digging efficiency.
- Paw/hand proportions shift: broader, more muscular, with reinforced wrist/ankle joints.
- The creature's resting pose changes to accommodate the enlarged claws — hands naturally curl rather than flatten.

**Environments Opened:**
- Rubble-blocked passages in Era 2 and 3 biomes become passable.
- The Root Cathedral's root labyrinth gains additional routes (digging around obstacles).
- Access to buried caches and hidden spaces throughout the game (many narrative fragments are behind diggable barriers).
- Combat advantage in close-range encounters.

**Tradeoff:**
- The enlarged claws reduce fine manipulation. If the game includes any interaction that requires delicate handling, the player must find alternatives.
- Climbing surfaces with the hardened claws produces audible scraping — stealth on vertical surfaces is compromised.

**Silhouette Impact:** Moderate. The enlarged claws are visible in all poses, especially in profile.

---

#### Tier 2: Vibration Sensing

**Mechanical Effect:**
- The creature detects vibrations through solid surfaces. Any creature moving on or through the ground within 20 meters is detected as a directional indicator, even through walls.
- Intensity of the vibration signal indicates size and speed of the source.
- The sense is passive and constant — it does not require activation and has no cooldown.
- The creature can also detect structural instability (floors about to collapse, ceilings about to fall) as a pre-emptive warning.

**Player Controller Hooks:**
- Passive detection: `vibration_sense` — displays directional indicators at screen edge for detected ground-contact creatures within 20m radius.
- Indicator intensity = creature mass * movement speed.
- Structural instability warning: surface tagged "unstable" within 10m triggers a subtle UI cue (screen-edge pulse) before failure.

**Visual Change:**
- Sensory structures develop along the creature's jaw, feet, and belly — areas in contact with surfaces. These appear as slightly raised, smooth patches with a different texture from surrounding skin.
- The creature's posture shifts: it presses closer to the ground in alert state, pressing sensory patches against the surface.
- Idle animation includes the creature pressing its jaw to the ground to "listen" — a distinctive behavioral tell.

**Environments Opened:**
- Underground navigation in total darkness becomes safer: the creature can "see" through vibration even without light or echolocation.
- Ambush predator detection: creatures that are motionless but touching the ground are detectable (they shift weight, breathe, fidget — all producing micro-vibrations).
- Structural assessment: the creature knows which floors will hold and which will not, transforming unstable building navigation from a guessing game to a skill.

**Tradeoff:**
- Sensitivity to vibration means vulnerability to vibration. Heavy impacts near the creature (falling debris, Corridor Herd stampedes, creature charges) cause a brief sensory overload (stun/flinch) as the vibration sense is overwhelmed.
- The creature avoids hard, vibration-conducting surfaces instinctively — movement on metal and stone surfaces triggers a slight hesitation in the animation (brief speed reduction on first contact with resonant surfaces).

**Silhouette Impact:** Minimal. Posture change and sensory patch texture are visible but do not change the outline significantly.

---

#### Tier 3: Compact Form

**Mechanical Effect:**
- The creature can compress its body to pass through spaces half its normal width. This is not a toggle — the creature automatically compresses when entering a tight space, and the player experiences a brief transition to a compressed movement mode.
- Compressed movement: 60% of normal speed, no jumping, no combat. The creature is committed to the passage.
- The creature's hitbox reduces when compressed, making it a smaller target (50% size reduction).
- Burrowing speed doubles (the compact form is more efficient at moving through dug tunnels).

**Player Controller Hooks:**
- Auto-trigger: creature enters `compact_state` when passage width is between 0.5x and 1.0x creature width.
- `compact_movement_speed: 0.6 * base`
- `compact_hitbox: 0.5 * normal`
- `dig_speed_modifier: 2.0` when in compact state.
- Jumping and combat inputs disabled in compact state.

**Visual Change:**
- The creature's body visibly compresses: ribs flex, limbs tuck, the spine curves. The transformation is animated and slightly unsettling — the creature looks like it shouldn't be able to fit, but it does.
- In compact form, the creature's movement animation is a rippling, almost serpentine motion — it pushes through tight spaces with its whole body.
- The creature's resting body plan adjusts: even when not compressed, the body appears more flexible, more loosely jointed. The skeleton is clearly adapted for deformation.

**Environments Opened:**
- Tight passages throughout the underground become accessible: maintenance ducts, cable runs, pipe interiors, cracks in walls, and narrow root passages.
- The Deep Archive's sealed sections may have ventilation ducts or pipe chases too narrow for normal passage — the compact form allows entry.
- Escape routes: any gap wide enough for half the creature's body becomes an exit. Many predator encounters can be escaped by finding a crack the predator cannot follow through.

**Tradeoff:**
- The loosened skeletal structure reduces structural integrity. The creature takes 20% more damage from crushing and impact sources.
- The compact form's movement restrictions (no jump, no combat) mean the creature is vulnerable during any tight-space transit. If a threat appears in a tight space, the creature must exit before it can respond.
- The flexible body plan reduces maximum exertable force: the creature cannot push or force open heavy obstacles as effectively (-30% to force-based interactions).

**Silhouette Impact:** Moderate normally (looser, more flexible posture). Major in compact form (a dramatically different shape).

---

#### Tier 4: Low-Light Vision

**Mechanical Effect:**
- The creature's eyes restructure for maximum light sensitivity. In any environment with any light source (even the faintest bioluminescence, starlight through cracks, or residual glow from distant sources), the creature sees at near-daylight clarity.
- Complete darkness (the Deep Archive's deepest sealed rooms — truly zero photons) still requires echolocation or bioluminescence. But in practice, almost every environment in the game has some light source, and this evolution makes it fully usable.
- Night vision mode is passive and permanent — no activation required, no cooldown, no stamina cost.
- The creature can read fine details in conditions where other organisms see only blackness: text on walls, subtle creature camouflage patterns, structural details.

**Player Controller Hooks:**
- `low_light_vision: true` — the rendering engine applies a brightness boost to all light sources within the player's view. Environments that were dark become navigable; environments that were dim become clear.
- The effect is calibrated so that "near-zero light" = "dim visibility" and "any measurable light" = "clear visibility." True zero-light environments remain dark.
- Fine detail rendering is enhanced: textures that were lost in darkness become readable.

**Visual Change:**
- Eyes become extremely large relative to the head. Irises are wide and reflective, catching and amplifying available light. In darkness, the eyes glow with reflected light — the creature's own eye-glow provides a faint supplementary light source.
- Pupils are permanently dilated, giving the creature an intense, nocturnal appearance.
- The head shape adjusts to accommodate the larger eyes, becoming slightly rounder.
- Overall color scheme darkens: the creature absorbs darker pigments to reduce visual contrast in the underground environments it inhabits.

**Environments Opened:**
- All underground environments become fully navigable as long as any light exists (and in the game world, between bioluminescence, residual glow, and distant sources, nearly every underground space has some light).
- Night-time surface exploration becomes trivial — starlight and moonlight provide full vision.
- Reading human artifacts in dark environments (the Deep Archive's preserved documents, the Hospital Quarter's medical records in dark wards) without needing to generate light.

**Tradeoff:**
- Bright light is painful. Daylight on the Salt Flats, the Canopy surface, or any brightly lit environment causes visual overload — the screen washes out to white, and the creature takes a stamina drain from the discomfort. The player avoids bright environments or squints through them.
- Flash effects (lightning, fire, bioluminescence pulses) cause brief blindness (longer than for un-adapted creatures, because the amplified light sensitivity amplifies the flash impact).
- The large, reflective eyes are the creature's most visible feature in the dark. Predators that hunt by eye-shine detection can spot the creature at extreme range.

**Silhouette Impact:** Major. The oversized eyes dominate the head profile. The creature is immediately recognizable as a deep-underground specialist.

---

### Armored Branch

**Environmental Trigger:** Sustained damage from hostile environments and aggressive creatures. Salt Flats, Migration Corridor (during herd activity), any biome with frequent combat encounters.

**Branch Theme:** The body learns to endure — to take damage and keep moving, to sacrifice speed for survival.

---

#### Tier 1: Carapace Growth

**Mechanical Effect:**
- Hard plates develop on the creature's back, shoulders, and head. Damage from physical attacks reduced by 30%.
- Resistance to environmental abrasion (salt wind, debris, rock falls) increased significantly.
- No movement penalty at this tier.

**Player Controller Hooks:**
- `physical_damage_modifier: 0.7`
- `abrasion_resistance: 0.8` (80% reduction in environmental abrasion damage)

**Visual Change:**
- Visible plating on the dorsal surfaces: shoulders, spine, back of head. The plates are dark, matte, and visibly dense.
- The creature's posture adjusts to protect plated areas: slightly hunched, head lowered, shoulders forward.
- Color shifts toward the environment's defensive palette — grays, browns, dark greens.

**Environments Opened:**
- Salt Flats become survivable for extended periods (salt wind abrasion resistance).
- Hostile biome traversal with frequent combat (Migration Corridor during predator activity) becomes more forgiving.

**Tradeoff:**
- Weight increases slightly. Ground movement speed reduced by 5%.
- Flexibility decreases marginally. Turning speed reduced by 10%.

**Silhouette Impact:** Moderate. The plated dorsal surfaces create a heavier, more angular outline.

---

#### Tier 2: Impact Resistance

**Mechanical Effect:**
- Physical damage further reduced (total 50% with carapace plates).
- Fall damage reduced by 40%.
- The creature can withstand impacts that would stagger or knockback other organisms: charge attacks, falling debris, herd stampede damage.
- Knockback resistance increased by 60%.

**Player Controller Hooks:**
- `physical_damage_modifier: 0.5`
- `fall_damage_modifier: 0.6`
- `knockback_resistance: 1.6` (60% reduction in knockback distance from all sources)

**Visual Change:**
- Carapace coverage extends: chest, flanks, and upper limbs develop plating.
- The plates interlock, creating a visible armor system that covers approximately 60% of the body.
- Joint areas remain flexible (visible skin between plates) but the overall impression is of a heavily protected organism.
- The creature's movement changes: heavier footfalls, more deliberate motion, a sense of mass and momentum.

**Environments Opened:**
- Falling hazards across the game become less lethal — the creature can survive drops that would kill un-armored builds.
- Migration Corridor herd encounters become survivable: the creature can withstand being caught in a stampede.
- Combat-heavy areas where direct conflict is unavoidable become more manageable.

**Tradeoff:**
- Ground speed reduced to 85% of base (cumulative with Tier 1: total 80% base speed).
- Climbing speed reduced by 30% (the armor is heavy and reduces grip surface area).
- Swimming speed reduced by 20% (the armor is not hydrodynamic).
- Stealth capability significantly reduced: the creature's heavy footfalls are audible, and its large, angular silhouette is visually conspicuous.

**Silhouette Impact:** Major. The creature is visibly armored. The angular plate outlines dominate the profile.

---

#### Tier 3: Retractable Appendages

**Mechanical Effect:**
- The creature can retract vulnerable appendages (limbs, tail, sensory structures) into the armored shell, reducing its profile and protecting soft tissue.
- "Turtle mode": full retraction reduces the creature to a compact, heavily armored shape. In this state: 90% physical damage resistance, no movement, no sensory input (the creature is blind and deaf inside its shell). The creature can sustain heavy damage in this state and survive encounters that would be fatal to any other build.
- Retraction and extension take 2 seconds each (the creature is vulnerable during transition).

**Player Controller Hooks:**
- New input: `retract_toggle`
- Retracted state: `physical_damage_modifier: 0.1`, `movement: disabled`, `sensory_input: disabled`, `visibility: internal_only` (screen goes dark, audio muffled).
- Transition time: 2 seconds in, 2 seconds out. Interruptible (if hit during transition, creature either completes retraction at reduced speed or aborts).

**Visual Change:**
- The armor coverage is now complete. The creature's body is nearly entirely plated.
- In retracted state, the creature is an ovoid armored shape — no visible limbs, head, or tail. It looks like a rock or debris pile.
- The retraction animation is dramatic: limbs fold in, plates slide over, the creature shrinks into itself.
- In the environment, a retracted creature is nearly indistinguishable from background debris (useful for passive camouflage).

**Environments Opened:**
- Survival in the most hostile encounter situations: overwhelming predator numbers, environmental catastrophes (cave-ins, explosions, herd stampedes), and boss-tier creature attacks.
- Passive camouflage in retracted state enables a new traversal strategy: the creature can wait, retracted, for threats to pass before emerging and continuing.
- Surviving environmental events that would kill any other build: total collapse, full submersion in lethal fluid (temporary — the sealed shell provides brief protection), direct hits from the most powerful creatures.

**Tradeoff:**
- The complete armor coverage means the creature has very limited flexibility. Ground speed is now 70% base. Climbing is barely possible (50% climbing speed, cannot climb overhangs). Swimming is severely compromised (30% swim speed — the creature sinks).
- The retracted state is blind and deaf. The creature cannot monitor its surroundings while protected. Emerging at the wrong time is dangerous.
- Transition time (2 seconds in and out) creates vulnerability windows that skilled predators can exploit.
- The creature is heavy. Structures that support normal creatures may not support this one. Fragile surfaces, thin ice, unstable branches — all become more dangerous underfoot.

**Silhouette Impact:** Dramatic. The creature is a tank. In retracted state, it is an unrecognizable armored ball. In extended state, it is the most heavily armored creature in the game.

---

#### Tier 4: Siege Form

**Mechanical Effect:**
- The creature can lock its limbs and charge in a straight line: a 5-second charge attack that deals massive damage to anything in its path and breaks through most physical barriers (sealed doors, rubble walls, weakened structures).
- The charge is committed: once started, the creature cannot turn or stop until the charge duration ends or it hits an immovable object.
- Charge damage: lethal to most mid-tier creatures. Significant damage to apex predators. Breaks through barriers rated "destructible."
- Self-damage: minor (the armor absorbs the impact).
- Cooldown: 45 seconds (the metabolic cost is significant).

**Player Controller Hooks:**
- New input: `siege_charge` — player aims direction, creature accelerates to 200% base speed in a straight line for 5 seconds.
- Steering disabled during charge. Charge ends on: timer expiry, collision with "immovable" object, or manual cancel (brief stumble animation on cancel).
- `charge_damage: high` (scaled to creature mass and speed).
- `barrier_break: true` for surfaces tagged "destructible."
- `self_damage: minimal` on collision with immovable objects.
- Cooldown: 45 seconds.

**Visual Change:**
- The creature's armor is now extreme: layered plates, reinforced head shield, splayed limbs with armored tread surfaces.
- During charge, the creature's posture drops into a full-forward position, head down, armored surfaces leading. Dust and debris spray from underfoot. The visual is unmistakable: a living battering ram.
- At rest, the creature's mass is its dominant feature. It is the largest, heaviest build in the game. Its footfalls leave visible impressions.
- Color is dark, matte, and geological: the creature looks like an animated piece of terrain.

**Environments Opened:**
- Sealed doors in the Deep Archive become breakable (this is one of only two ways to open them — the other is finding the key).
- Rubble barriers throughout the game become passable.
- Predator encounters become solvable through direct force — the creature can charge through or into threats that other builds must evade.
- The charge covers ground fast, providing a burst-mobility option that compensates partially for the low base speed.

**Tradeoff:**
- Base movement speed is now 60% — the slowest build in the game. The creature lumbers.
- Climbing is effectively impossible. The creature's mass and bulk prevent meaningful vertical traversal.
- Swimming is lethal at depth — the creature sinks and cannot surface without a slope to walk up.
- The siege charge is a commitment. Charging in the wrong direction, off an edge, or into an unexpected obstacle can create worse situations than the one the charge was meant to solve.
- Gliding is impossible. The creature is far too heavy for any membrane to support.
- The creature's weight makes it a constant structural threat to its own environment. Floors collapse, bridges sag, thin ice breaks. The creature's traversal options narrow to the most robust paths.

**Silhouette Impact:** Extreme. The creature is a fortress. No other build looks anything like this. The silhouette communicates "unstoppable" and "immobile" simultaneously.

---

### Sensory Branch

**Environmental Trigger:** Passive accumulation across all biomes. The Sensory Branch does not require a specific environment — it develops in response to the creature's total breadth of environmental experience. Time spent in diverse biomes fills the sensory adaptation meter.

**Branch Theme:** The body learns to perceive — to sense more, further, finer. The Sensory Branch is the generalist counter to the specialist branches.

---

#### Tier 1: Enhanced Hearing

**Mechanical Effect:**
- Audio detection range doubles. The creature can hear creature movements, environmental sounds (water, wind, structural settling), and ambient activity from twice the normal distance.
- Directional audio precision improves: the player can determine the direction and approximate distance of any sound within detection range.
- New UI element: subtle directional sound indicators at screen edges when significant sounds are detected outside the visible area.

**Player Controller Hooks:**
- `audio_detection_range: 2.0 * base`
- UI overlay: directional indicators for detected sounds outside camera view. Indicator intensity = sound volume/proximity.

**Visual Change:**
- Ear structures enlarge and become more mobile. The creature's ears visibly track sounds — rotating toward detected noise.
- The creature's alert posture changes: ears forward, head slightly tilted, body still. The "listening" pose is distinctive.

**Environments Opened:**
- Ambush predator detection via sound cues (breathing, movement, weight shifts) before visual confirmation.
- Environmental hazard prediction (structural groaning before collapse, water rushing before flood).
- Social creature tracking (pack predator vocalizations reveal their positions and movements).

**Tradeoff:**
- Loud environments cause mild distraction (reduced focus in high-noise areas like the Migration Corridor during herds, or Transit Corridors during Rush Hour Swarms).

**Silhouette Impact:** Minimal. Enlarged ears visible but do not dramatically change the outline.

---

#### Tier 2: Chemical Detection

**Mechanical Effect:**
- The creature can detect chemical signatures in the air and on surfaces.
- Creatures leave scent trails that the player can follow or avoid. Trail intensity indicates recency and creature type.
- Environmental chemistry becomes readable: toxic zones are detected before entering them, pharmaceutical concentrations in the Hospital Quarter are mapped by scent, chemical composition of water is assessable before drinking or entering.
- Food sources are detectable by scent at 30-meter range.

**Player Controller Hooks:**
- New passive: `chemical_detection` — displays scent trails as visible overlays on surfaces (color-coded by creature type, intensity by recency).
- Environmental chemistry: areas tagged "toxic" or "chemical" display a visible aura at their boundaries before the player enters.
- Food detection: food-tagged objects display a subtle indicator when within 30m.

**Visual Change:**
- Nasal and oral structures develop: the creature's snout/face elongates slightly, nostrils widen and become more prominent.
- Visible Jacobson's organ equivalent: the creature flicks its tongue or opens its mouth slightly when "tasting" the air. This is a frequent idle animation.
- Whisker-like structures develop around the nose and mouth — sensory hairs that detect airborne chemicals.

**Environments Opened:**
- Tracking creatures across biomes (following scent trails).
- Safe path-finding through chemical hazard zones (detecting toxic boundaries before contact).
- Finding hidden food sources and resource deposits.
- Narrative content: chemical signatures on human artifacts reveal information (what substances were stored here, what chemicals were used, whether biological activity has been recent).

**Tradeoff:**
- The creature's own chemical signature becomes more complex (the sensory organs excrete metabolic byproducts). Creatures that hunt by chemical detection find the player more easily (+30% detection range for chemical-tracking predators).
- Overwhelming chemical environments (Industrial Wetlands at peak chemical fog) can overload the sense, causing temporary nausea (brief stamina drain and movement reduction).

**Silhouette Impact:** Moderate. The elongated face and whisker structures change the head profile noticeably.

---

#### Tier 3: Electrical Field Sensing

**Mechanical Effect:**
- The creature detects bioelectrical fields generated by living organisms. All creatures within 15 meters are detected regardless of concealment (underwater, buried, camouflaged, hidden behind cover). The detection reveals position and general size.
- Active electrical sources (still-running servers, live wiring, electrochemical reactions) are detectable and mappable.
- New mechanic: the creature can detect the bioelectrical stress signatures of creatures, reading "intention" — a creature about to attack produces a different electrical signature than one that is calm. This provides a 1-second pre-warning before any creature attack within range.

**Player Controller Hooks:**
- Passive detection: `electrical_sense` — displays all bioelectrical sources within 15m as subtle indicators, penetrating concealment.
- Active electrical source detection: objects tagged "electrical" display visible aura.
- Attack pre-warning: creatures within 15m that enter "attack" state trigger a 1-second warning indicator (directional flash at screen edge, specific to the attacking creature).

**Visual Change:**
- Lateral line organs develop along the creature's flanks — visible as a stripe of specialized tissue from head to tail.
- The lateral line is slightly raised and has a different texture from surrounding skin — smooth, dark, and densely innervated.
- In the presence of strong electrical fields, the lateral line tissue visibly responds — slight twitching or color shift.

**Environments Opened:**
- Underwater environments become fully transparent to sensing — all aquatic creatures are detectable regardless of water clarity.
- Ambush predators across all biomes lose their primary advantage: the creature knows they are there before they strike.
- Still-active electrical infrastructure (Server District, Deep Archive power systems) becomes mappable, revealing safe paths through electrified areas.
- The attack pre-warning transforms combat from reactive to proactive — the creature always has 1 second to prepare for incoming attacks.

**Tradeoff:**
- Strong electrical fields cause sensory overload. Proximity to active electrical infrastructure (Server District server rooms, live substations in the Grid Forest during electrical storms) causes disorientation (brief screen distortion, movement impairment).
- The lateral line organs are exposed and vulnerable. Attacks that specifically target the flanks deal 30% additional damage.
- The creature's own bioelectrical output increases (the sensing organs generate their own field). Electrically-sensitive predators (Tunnel Eels, certain aquatic creatures) detect the creature from greater range.

**Silhouette Impact:** Moderate. The lateral line creates a visible stripe along the body, and the overall profile becomes slightly wider at the flanks.

---

#### Tier 4: Synaptic Acceleration

**Mechanical Effect:**
- The creature's neural processing speed increases dramatically. In gameplay terms, this manifests as a "slow time" ability: the player can activate a state where the game world appears to slow to 50% speed for 5 seconds, allowing precise action in situations that would normally be too fast to react to.
- Passive: general reaction speed improves. The 1-second attack pre-warning from Electrical Field Sensing extends to 2 seconds. The creature's dodge window increases (more frames of invulnerability during dodge animations).
- Passive: environmental assessment is faster. The creature processes visual, audio, and chemical information more quickly — in gameplay, this means detection indicators from all other senses appear 0.5 seconds earlier than they would otherwise.

**Player Controller Hooks:**
- New input: `synaptic_acceleration` — activates slow-time mode. World simulation at 50% speed for 5 seconds. Player input response at full speed (the player controls their creature normally while the world is slowed). Cooldown: 60 seconds.
- `dodge_invulnerability_frames: +30%`
- `attack_prewarning_time: 2.0 seconds` (upgraded from Tier 3's 1.0)
- All detection indicators from other senses appear 0.5 seconds earlier.

**Visual Change:**
- The creature's head develops a slightly enlarged cranial structure — visible as a smoother, more domed forehead and skull.
- Eye structures change: pupils become more complex, with visible multiple-layer iris structures suggesting enhanced optical processing.
- During slow-time activation, the creature's eyes glow briefly and the creature's movements appear hyper-precise — animations become smoother and more controlled.
- Overall, the creature's head and face become more "intelligent-looking" — larger head-to-body ratio, complex eyes, alert posture.

**Environments Opened:**
- Combat encounters that were previously too fast to manage become tractable. Multiple simultaneous attackers can be individually addressed.
- Environmental hazards with narrow timing windows (steam vents, collapsing structures, timed mechanisms) become safely navigable.
- The combination of all Sensory Branch tiers creates a creature that perceives the world at a level of detail no other build can match — not stronger, not faster, not tougher, but more aware.

**Tradeoff:**
- The enhanced neural system demands energy. Stamina drain during synaptic acceleration is extreme — the 5-second window costs approximately 40% of the stamina bar. The creature cannot sprint immediately after using it.
- The slow-time cooldown is the longest in the game (60 seconds). The ability must be used judiciously.
- The creature's heightened perception can become a liability in overwhelming sensory environments: too many creatures, too many sounds, too many chemical signatures at once cause "information overload" — a brief period of reduced sensory effectiveness as the brain deprioritizes to recover.
- The enlarged cranial structure is a vulnerable point. Head-targeting attacks deal 20% bonus damage.

**Silhouette Impact:** Moderate. The enlarged cranium and complex eye structures change the head profile. The creature looks observant and aware — which it is.

---

## Soft-Lock Prevention

### The Problem

Rewild's permanent evolution system creates a fundamental design risk: a player could evolve in a direction that makes it impossible to complete the game. If the critical path requires aquatic traversal and the player has invested all 12 mutation slots in thermal and armored branches (making swimming nearly impossible), the player is soft-locked — unable to progress, unable to revert.

This is unacceptable. Permanent consequences for choices is a design pillar. Permanent inability to complete the game is not a consequence — it is a failure.

### Critical Path Requirements

The game's critical path — the minimum sequence of objectives required to reach the ending — passes through specific biomes that impose specific environmental requirements:

1. **Water traversal** — at least one critical-path route requires submerging for at least 30 seconds.
2. **Vertical traversal** — at least one critical-path objective is above ground level, requiring climbing or equivalent.
3. **Darkness navigation** — at least one critical-path section is in complete or near-complete darkness.
4. **Toxin exposure** — at least one critical-path route passes through a toxic zone.
5. **Barrier passage** — at least one critical-path barrier requires either force (breaking through) or compactness (squeezing through) to pass.

No other environmental requirement is on the critical path. Heat, cold, salt, electrical, chemical, deep-water, high-altitude, and canopy requirements are reserved for optional content and non-critical biomes.

### Universal Mutations

To ensure every build can meet the critical path requirements, the game includes a set of "universal mutations" that are available to all builds regardless of branch investment.

Universal mutations are not part of any branch. They are weaker versions of branch-specific mutations that trigger when the player has been in an environment long enough to need the adaptation and has no branch mutation that addresses it. They cost a mutation slot but have lower trigger thresholds.

| Critical Need | Universal Mutation | Effect |
|--------------|-------------------|--------|
| Water traversal | Extended Breath | Holds breath for 90 seconds (vs. default 30). Not infinite, but enough for critical-path water sections. |
| Vertical traversal | Rough Climbing | Can climb rough vertical surfaces at 30% movement speed (slower than Gecko Pads but functional). |
| Darkness navigation | Dark Adjustment | Slowly adapts to darkness (30-second adjustment period, then dim vision). Not as good as Low-Light Vision, but functional. |
| Toxin exposure | Basic Toxin Filter | Reduces toxin damage by 30% (vs. Spore Membrane's 50%). Enough to survive critical-path toxin zones with health management. |
| Barrier passage | Brute Force / Squeeze | One or the other triggers depending on the player's build: heavy builds get "Brute Force" (can break destructible barriers slowly). Light builds get "Squeeze" (can fit through gaps slightly smaller than normal body width). |

Universal mutations trigger automatically — the player does not choose them. They appear when the critical path demands an adaptation the player does not have. They are narratively framed as the body's emergency response: a less efficient, less elegant adaptation forced by immediate need rather than sustained pressure.

### Graph Validator Design

During development, a graph validator runs continuously against the biome connectivity and gating data to verify that:

1. **For every possible combination of 12 mutations (from any branches), there exists at least one complete path from the starting biome to the ending objective.** If a combination fails this test, the validator identifies which barrier is impassable and flags it for redesign.

2. **Universal mutations are sufficient to bridge any gap between the player's actual build and the critical path requirements.** The validator tests each universal mutation against the specific environmental challenges it must address, verifying that a player with only universal mutations (no branch investment at all) can still complete the critical path.

3. **No combination of mutations creates unintended traversal shortcuts that bypass critical narrative content.** The validator checks that the intended narrative sequence is preserved even under extreme build combinations.

The validator outputs a pass/fail report for each combination, with failure reports including specific barrier identification and suggested remediation.

### Fallback Mechanics

In addition to universal mutations, the game includes environmental fallback mechanics that provide alternative solutions to gated areas:

- **Alternative routes:** Every gated passage has at least one alternative route that uses a different adaptation. If the primary route requires aquatic traversal, an alternative route using climbing or burrowing exists (even if it is longer or more dangerous).
- **Environmental tools:** Some biomes contain environmental features that can substitute for evolutionary adaptations: natural air pockets in submerged areas (reducing the breath requirement), fallen trees creating natural bridges over vertical drops (reducing the climbing requirement), light-producing environmental features in dark areas (reducing the darkness navigation requirement).
- **NPC cooperation:** Certain non-hostile creatures in the game world can be observed and followed through environmental barriers. A creature that is adapted to water may reveal an underwater route through its behavior. A climbing creature may demonstrate a wall route. The player who observes and follows learns the environment's solutions.

---

## Evolution Visual Language

### The Transformation Moment

The evolution transformation is one of the game's signature visual events. It follows a precise visual script:

**Frame 1-30 (1 second): Onset**
The creature stutters. Its current animation interrupts. The body tenses — muscles lock, limbs stiffen. A visible wave of tension propagates from the core outward. The creature's color shifts subtly: a flush of the mutation branch's associated color (warm for thermal, blue for aquatic, green for fungal, etc.) washes through the skin.

**Frame 31-90 (2 seconds): Transformation**
The mutation manifests. This is specific to the mutation type:
- Structural mutations (fins, membranes, tails, armor): the new structure pushes outward from beneath the skin. The skin stretches, splits in a controlled way, and the new feature emerges. There is no blood — this is biological, not violent. The emerging structure is initially soft and pale, then rapidly hardens and takes on color.
- Textural mutations (pads, membranes, carapace): the existing skin ripples and changes. Texture shifts propagate across the affected area like a wave. Color changes follow the texture change.
- Sensory mutations (eyes, ears, lateral line): the affected organs swell, reshape, and settle into their new form. The change is smooth and organic — a face restructuring itself.
- Internal mutations (gill-lungs, lightweight bones, pressure resistance): the external manifestation is subtler — the body's overall proportions shift, posture adjusts, the creature's breath pattern changes. The internal change is implied by the external adjustment.

**Frame 91-150 (2 seconds): Completion**
The new mutation stabilizes. The creature tests it involuntarily: a new fin fans, a new tail curls, new eyes blink. The creature's base palette adjusts to incorporate the mutation's color influence. The creature resumes its idle animation with the mutation integrated into all future animations.

**During the entire transformation:** The world continues. The camera does not change. The music does not swell. No UI appears until the selection interface at the end. The world's indifference to the creature's transformation is the point.

### Biome Color Absorption

The creature's palette is not static. It absorbs the dominant colors of the biomes it evolves in. This is the visual implementation of "You Become What You Survive."

**The mechanic:**
Each biome has a "palette influence" — a set of 3-5 colors that represent its visual identity. When the creature evolves a mutation in a biome, the mutation's associated body region permanently incorporates the biome's palette influence. The result is that a creature's color map is a literal record of where it evolved.

**Examples:**
- A creature that evolved Gill-Lungs in the flooded Transit Corridors has gill tissue in the amber-and-teal of corroded subway tile and tunnel water.
- A creature that evolved Heat Dissipation Fins in the Server District has fins in the orange-and-blue of indicator LEDs and bioluminescent fungi.
- A creature that evolved Gecko Pads in the Root Cathedral has pad surfaces in the deep brown of ancient bark and the moss-green of the Cathedral floor.

By endgame, the creature is a patchwork of every biome that shaped it. No two players' creatures look the same, because no two players evolve in the same biomes in the same order.

### Silhouette Progression

The creature's silhouette is the most important visual communication tool. Other players, creatures, and the game's own systems react to the creature's outline, not its color or texture. The silhouette tells the story at a glance.

**Starting silhouette:** A compact, generalist quadruped. Unremarkable. Medium size, medium proportions, no distinctive features. The blank canvas.

**Branch silhouette signatures:**
- **Thermal:** Fin protrusions along spine and shoulders. Visible vascular glow. The creature is spiky and warm.
- **Aquatic:** Webbed limbs, streamlined head, broader torso. The creature is smooth and hydrodynamic.
- **Fungal:** Asymmetric growths, root-like structures, bioluminescent patches. The creature is organic and irregular.
- **Arboreal:** Long limbs, visible membranes, prehensile tail, lean frame. The creature is angular and airy.
- **Burrowing:** Enlarged claws, compact frame, large eyes or closed-eye posture. The creature is dense and low.
- **Armored:** Plated surfaces, heavy frame, broad stance. The creature is massive and geometric.
- **Sensory:** Enlarged head, prominent ears/eyes/whiskers, alert posture. The creature is observant and precise.

**Multi-branch silhouettes:** A creature with mutations from multiple branches combines their silhouette elements. This creates unique hybrid forms:
- Aquatic + Arboreal: webbed limbs with membrane flaps. Amphibious glider.
- Thermal + Armored: finned and plated. A furnace in a shell.
- Fungal + Sensory: bioluminescent with enlarged sensory organs. An alien observer.
- Burrowing + Armored: massive claws on a heavily plated frame. A living excavator.

The silhouette system ensures that every build is visually distinct and that the creature's evolutionary history is readable at any distance.

### The Ghost Silhouette Technique

When the creature evolves, its previous silhouette is briefly visible as a translucent "ghost" overlapping the new form for approximately 3 seconds after transformation completes. This ghost shows the creature's immediately-prior form, creating a visual before/after comparison.

The ghost fades quickly. It is not stored or retrievable. The creature has moved on. The ghost is a reminder of what the creature was — and by its fading, a statement that the creature will not be that again.

The ghost silhouette technique also functions at a meta-narrative level: the player sees the ghost of the form they are losing. The permanence of evolution is communicated viscerally: that shape is gone. The new shape is what remains.

---

## Evolution and Narrative

### The Central Irony

The player creature is doing what humans could not: adapting.

The entire narrative of Rewild is built on the premise that humanity devolved because it outsourced its adaptive capacity to technology. Instead of learning new skills, humans asked AI. Instead of developing new capabilities, humans automated. Instead of evolving in response to environmental pressure, humans engineered their environment to eliminate the pressure.

The player creature does the opposite. It enters hostile environments and stays. It endures pressure and transforms. It develops new capabilities through direct biological response. It does not ask for help; it becomes the help it needs. Every mutation is a statement: the body can solve this problem without intermediary technology.

This is not framed as superiority. The creature is not "better" than humans were. It is simply doing what biology does when given time and pressure. Humans did it too, for millions of years, before they found a way to stop. The creature's evolution is not triumphant — it is ordinary. That is the point. Evolution is what happens when organisms participate in the world instead of abstracting themselves from it.

### Evolution as Thematic Counterpoint to Devolution

The game's background narrative describes human cognitive devolution across generations:

- **Generation 1:** Used AI as a tool. Maintained independent capability.
- **Generation 2:** Used AI as a partner. Independent capability began atrophying.
- **Generation 3:** Used AI as a replacement. Independent capability became unnecessary.
- **Generation 4:** Could not function without AI. Independent capability was extinct.
- **Generation 5:** Could not understand what had been lost. The concept of independent capability was meaningless.

The player creature's evolution mirrors this progression in reverse:

- **Early game:** The creature is a generalist with no specialized capability. It is vulnerable, undifferentiated, and reliant on caution.
- **Mid game:** The creature has developed initial specializations. It can survive in environments that were previously lethal. It has gained agency through adaptation.
- **Late game:** The creature is highly specialized. It has capabilities that no other organism in its environment possesses. It has become something new — not through design, but through lived experience.
- **Endgame:** The creature is a unique organism, shaped entirely by its specific survival history. No two creatures are the same. Each one is a complete argument for the power of biological adaptation.

The creature's evolution is the mirror image of human devolution. Where humans lost capability through dependency, the creature gains capability through exposure. Where humans became uniform through standardization, the creature becomes unique through experience. Where humans abstracted themselves from the environment, the creature embeds itself in the environment so deeply that it becomes part of it.

### The Pharmaceutical Echo

The Hospital Quarter's narrative about pharmaceutical dependency creates a specific resonance with the evolution system. The humans medicated themselves to manage symptoms. The creature evolves to eliminate causes.

When the player evolves Toxin Resistance in the Hospital Quarter — standing in the ruins of a civilization that used pills to manage its discomfort — the game is making its most pointed thematic statement: the pharmaceutical approach (manage the symptom, don't address the cause) is the cognitive approach (manage the problem, don't develop the capability). The creature's body develops actual resistance. It doesn't need a pill. It changed.

### The Deep Archive Encounter

The narrative climax of the evolution system occurs in the Deep Archive. When the player — in whatever heavily-evolved form their journey has produced — enters the most preserved human space in the game and encounters human records, the contrast between the creature's evolved body and the static human artifacts creates the game's emotional peak.

The creature that reads (or senses, or detects, or resonates with) the time capsule messages in the Deep Archive has been shaped by the world those message-writers failed to adapt to. The creature's body is proof that adaptation was possible. The messages are proof that the humans knew they were failing. The gap between the two is the game's thesis.

The creature does not comment on this. The creature has no dialogue. The creature reads the message with its evolved senses and moves on. The world does not care about the irony. Only the player cares. And the player cares because they are the one who chose every mutation that shaped the creature that is standing in the vault. The player adapted. The humans in the messages did not. The player succeeded where the humans failed.

That realization — experienced, not stated — is the evolution system's narrative purpose.

### Color as Memory

The creature's biome-absorbed color palette serves a final narrative function in the Deep Archive. The creature standing in the warm, muted, institutional colors of the preserved human space is itself colored by every biome it survived: the blues of underwater, the greens of canopy, the purples of fungal depths, the oranges of thermal zones. The creature is a living Decay Gradient — cool, saturated biological colors carried into the last warm, muted human space. The creature's body is the present, standing in the past. Its colors say: the world moved on. Its presence says: something new moved in.
