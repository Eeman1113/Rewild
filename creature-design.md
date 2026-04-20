# Rewild -- Creature Design Document

> Every creature in this world exists for itself, not for the player.
> The player is an intruder in a functioning ecosystem.
> The ecosystem does not pause, adjust, or accommodate.

This document defines the ecological framework, behavioral AI philosophy, creature archetypes, player creature design, devolved human design, and creature interaction systems for Rewild. Every creature in the game must feel like it belongs to a real ecosystem -- not placed for the player's entertainment but existing as part of an interconnected biological web.

---

## 1. Ecological Framework

The world of Rewild is a fully functioning post-human ecology. The ruin environment has created novel biomes with unique selection pressures, producing creatures that are adapted to a landscape of concrete, steel, glass, and reclaimed earth. The ecosystem is not a backdrop. It is the primary system of the game.

### 1.1 Food Web Structure

The food web is organized into five trophic levels, each with distinct gameplay implications:

**Producers (Trophic Level 1)**
- Dense vegetation: forests, meadows, algal blooms in flooded ruins, fungal networks in underground spaces
- Photosynthetic organisms dominate surface biomes; chemosynthetic organisms (feeding on degrading synthetic materials and mineral leaching from concrete) dominate underground biomes
- Producers are not "creatures" in the behavioral AI sense but are modeled as living systems that grow, spread, compete for light, and respond to seasonal cycles
- Gameplay role: Cover, traversal surfaces, environmental hazards (toxic species, thorned barriers), and the visual fabric of the world

**Primary Consumers (Trophic Level 2)**
- Herbivores and detritivores: creatures that eat plants, fungi, algae, and decaying organic matter
- These are the most abundant visible creatures in the world
- Behavioral emphasis: foraging patterns, herd dynamics, vigilance behavior (watching for predators while eating)
- Gameplay role: Ambient life, food source for the player creature and predators, indicators of biome health and predator presence (a clearing with no grazers means a predator is nearby)

**Secondary Consumers (Trophic Level 3)**
- Mesopredators and omnivores: creatures that eat primary consumers and sometimes plants
- The player creature begins at this level
- Behavioral emphasis: opportunistic hunting, scavenging, risk assessment (is the prey worth the energy expenditure and injury risk?)
- Gameplay role: Competition with the player, dynamic encounters, food chain intermediaries

**Apex Predators (Trophic Level 4)**
- Creatures at the top of the food chain in their respective biomes
- Rare, powerful, territorial, and dangerous
- Behavioral emphasis: energy conservation (apex predators rest most of the time), territorial patrol, selective hunting (targeting weak/isolated prey), avoidance of unnecessary conflict
- Gameplay role: Major threats, territory controllers, ecosystem shapers (their presence or absence changes the behavior of everything below them)

**Decomposers (Cross-level)**
- Fungi, bacteria, insects, and specialized scavengers that break down dead organic matter
- Present everywhere but concentrated in moist, dark environments (flooded ruins, underground, dense forest floor)
- Behavioral emphasis: Aggregation at carcass sites, cyclical activity patterns
- Gameplay role: Environmental atmosphere, timing indicators (a fresh carcass means a predator was here recently; a picked-clean skeleton means the area is heavily trafficked), and in some cases direct threats (aggressive insect swarms, toxic fungal clouds)

### 1.2 Energy Flow Through Biomes

Each biome has a characteristic energy budget that determines creature density, diversity, and behavior:

- **Overgrown Urban (high energy):** Dense vegetation, abundant water from broken infrastructure, rich substrate from decomposing human materials. Supports the highest creature density and diversity. Complex, multi-layered food webs.
- **Flooded Ruins (high energy, specialized):** Aquatic and semi-aquatic ecosystems in submerged lower levels of buildings, flooded subway systems, and broken dam watersheds. High productivity but limited to aquatic-adapted species. Dangerous for terrestrial creatures including the player.
- **Deep Forest (moderate energy):** Mature forest that has grown over former suburbs and low-density development. Canopy stratification creates vertical habitat layers. Moderate creature density with emphasis on arboreal species.
- **Exposed Highlands (low energy):** Hilltops, collapsed high-rises, and elevated terrain with thin soil and high wind exposure. Low vegetation, sparse creature populations, but specialized species adapted to the conditions. Raptors, alpine grazers, wind-dispersed plants.
- **Underground (low energy, specialized):** Subway tunnels, basements, parking garages, utility corridors. Near-total darkness. Chemosynthetic base. Cave-adapted species with unique sensory adaptations. Extremely dangerous. Low density but high individual threat level.
- **Server Farm Zones (anomalous energy):** Areas around still-functioning AI infrastructure. Waste heat creates tropical microclimates. Electromagnetic fields may affect creature behavior. Unique ecology found nowhere else. The energy source is artificial and finite (hardware degradation will eventually end it), creating an ecologically unstable zone.

### 1.3 Population Dynamics

Creature populations are not static. They fluctuate based on:

- **Seasonal cycles:** Some species migrate, hibernate, or estivate. Prey availability changes with seasons, altering predator behavior.
- **Predator-prey oscillation:** Classic Lotka-Volterra dynamics. When prey populations boom, predator populations follow with a delay. When predators over-consume, prey crashes and predators decline. The player can observe these cycles over a playthrough.
- **Disturbance events:** The player's actions can trigger local population effects. Killing an apex predator in a territory releases mesopredator populations. Depleting a prey species forces predators to hunt elsewhere (including potentially targeting the player).
- **Ecological succession:** Long-term environmental changes (a dam breaking, a fire, a building collapse) alter habitat and shift community composition.

The game does not need to simulate continent-scale population ecology. It needs to simulate local-scale dynamics in the areas the player inhabits, with plausible off-screen population behavior handled through simpler models.

### 1.4 Territorial Behavior

Territory is a fundamental organizing principle:

- **Apex predators** maintain large territories (multiple game "zones"). The player enters and exits predator territories as they move through the world. Knowing territorial boundaries is survival knowledge.
- **Mesopredators** maintain smaller territories, often within the gaps of apex predator territories. They avoid apex predators and compete with each other.
- **Prey species** have home ranges rather than defended territories. They move through areas based on resource availability and predator avoidance.
- **Territorial markers** are visible in the environment: scent marks (visual effects on terrain), scratch marks, vocal calls, scat, and behavioral displays. The player creature can learn to read these markers as part of the mastery curve.
- **Territorial disputes** between conspecifics are observable events. Two predators contesting a boundary engage in ritualized display behavior that can escalate to combat. These are not scripted events -- they emerge from the territorial AI system.

### 1.5 Migration Patterns

Some species move through the world on predictable schedules:

- **Seasonal grazers** that follow vegetation growth patterns, moving through the world in loose herds
- **Breeding migrations** to specific locations (a flooded ruin that serves as a spawning ground, a cliff face used for nesting)
- **Predator following** -- some predators track migrating prey herds rather than maintaining fixed territories
- **Disturbance displacement** -- creatures fleeing a local disaster (building collapse, flood, fire) moving through adjacent areas and disrupting local ecology temporarily

Migration creates dynamic encounter variety. The same location may host different creature communities at different times.

---

## 2. Behavioral AI Philosophy

### 2.1 Core Principle: Need-Driven Behavior

Every creature in Rewild operates on a need-satisfaction model. Behavior is not scripted, patrolled, or randomized. It emerges from the creature's current state and the environment's current conditions.

**The Five Needs:**
1. **Energy (hunger):** Depletes over time. Drives foraging, hunting, and scavenging behavior. When energy is critically low, creatures take greater risks.
2. **Safety:** Assessed continuously. Drives vigilance, flight, hiding, and defensive behavior. When safety is threatened, all other needs are deprioritized.
3. **Rest:** Accumulates as a need during activity. Drives shelter-seeking and sleep behavior. Resting creatures are vulnerable and choose rest locations accordingly.
4. **Homeostasis (comfort):** Temperature regulation, hydration, environmental tolerance. Drives movement toward favorable microclimates and away from environmental hazards.
5. **Reproduction:** Periodic drive. Triggers mate-seeking, courtship display, nesting, and offspring care. Not present in all individuals at all times.

At any moment, a creature's behavior is determined by its most pressing unsatisfied need, modulated by its current safety assessment. A hungry creature will hunt -- unless a predator is nearby, in which case safety overrides hunger. A tired creature will rest -- unless it is starving, in which case energy overrides rest.

### 2.2 Behavioral State Machine

Each creature operates through the following behavioral states. Transitions between states are driven by the need-satisfaction model, not by timers or scripts:

**Forage/Hunt:** Actively seeking food. Herbivores browse, moving between food patches. Predators search, stalk, and pursue. Scavengers patrol for carcasses. This is the default active state for most creatures most of the time.

**Flee:** Triggered by a perceived threat exceeding the creature's fight threshold. The creature moves away from the threat using species-appropriate evasion: sprinting, diving, climbing, burrowing, or freezing (for ambush-adapted prey that relies on camouflage). Flight continues until the creature reaches a safe distance or a known refuge.

**Fight/Defend:** Triggered when fleeing is not viable (cornered, defending nest/offspring, territorial boundary) or when the threat is within a fightable range (smaller than the creature, injured, outnumbered by the creature's group). Fight behavior varies by species: direct assault, threat display, venomous strike, mobbing.

**Rest:** The creature finds a suitable rest location (shelter, elevated perch, dense cover) and enters a low-activity state. Sensory alertness is reduced but not eliminated. Resting creatures startle more easily and flee more readily than active creatures.

**Migrate:** Directional movement toward a distant goal (seasonal destination, breeding ground, new territory). The creature is less responsive to local stimuli during migration, making it more predictable but also more likely to blunder into or through dangerous areas.

**Territorial Display:** Ritualized behavior marking and defending territory boundaries. Vocalizations, scent marking, physical displays. Escalates to combat only if the intruder does not retreat. Most territorial encounters are resolved through display, not violence.

**Nest/Den:** Stationary behavior centered on a fixed location. Nest-building, offspring care, incubation. The creature is highly defensive of this location and will fight threats it would normally flee.

**Social:** Grooming, play, affiliative behavior within social groups. Serves stress reduction and social bonding functions. Observable in social species during low-stress periods.

### 2.3 Creatures Exist for Themselves

This is the design commandment. No creature in Rewild exists to serve a gameplay function. No creature is "the enemy in zone 3." No creature spawns when the player arrives and despawns when the player leaves. The world is simulated (within practical performance limits) whether or not the player is present.

**What this means in practice:**
- Creatures have daily routines that continue with or without the player
- The player may arrive at a location and find it empty because the local predator is on the far side of its territory. Or arrive and find it swarming because a prey herd is migrating through.
- Creatures do not scale to the player's level. A newly started player who wanders into apex predator territory will die. This is not unfair. It is ecology.
- The player can observe creature behavior to learn patterns and make predictions. This IS the core skill progression. Knowledge replaces levels. Understanding replaces equipment.
- Creatures react to the player as they would to any animal of comparable size and threat level. They do not have special "player detection" behavior. They treat the player creature as a member of the food web.

### 2.4 Behavioral Signatures

Each creature species has a unique behavioral signature -- a combination of traits that makes it recognizable and learnable:

- **Activity pattern:** Diurnal, nocturnal, crepuscular, or cathemeral. When is the creature active?
- **Alertness profile:** How far does it detect threats? Through what senses? How quickly does it react?
- **Aggression threshold:** How much provocation before it fights? Does it escalate gradually or attack without warning?
- **Social structure:** Solitary, pair-bonded, small group, herd, colony. How does the presence of conspecifics modify behavior?
- **Habitat preference:** Where does it spend most of its time? What terrain does it prefer for different activities?
- **Response to novelty:** How does it react to unfamiliar stimuli (including the player on first encounter)? Curious, cautious, aggressive, indifferent?

The player learns these signatures through observation and (often painful) experience. This learning is the mastery curve. A veteran player reads the environment like a naturalist -- recognizing warning signs, predicting behavior, and making informed decisions about when to engage, avoid, or exploit.

---

## 3. Creature Archetypes

The following are design specifications for specific creature concepts. Each creature is defined by its ecological role, behavioral profile, and relationship to the ruin environment. Names are working titles. Visual design references the shape language from the art bible (angular = dangerous, rounded = passive, asymmetric = unpredictable).

### 3.1 Apex Predators

#### 3.1.1 The Castellan

**Ecological role:** Apex predator, overgrown urban biome
**Primary biome:** Dense ruin environments -- collapsed buildings, overgrown city blocks, structural mazes
**Body type:** Large quadruped, somewhere between a big cat and a monitor lizard. Heavily muscled forelimbs, flexible spine, low center of gravity. Built for navigating cluttered, multi-level terrain.
**Size:** 3-4 meters nose to tail. Significantly larger than the player creature.
**Behavioral traits:**
- Territorial. Maintains a domain centered on a major ruin complex. Patrols on a roughly 3-day cycle, covering the full territory.
- Ambush-transitional hunter. Uses the ruin geometry for concealment. Positions itself above prey on collapsed structures and drops. If the ambush fails, pursues for a short distance before breaking off (energy conservation).
- Solitary except during mating. Two Castellans in proximity means territorial dispute.
- Marks territory by clawing deep grooves into concrete and stone surfaces. These marks are distinctive and readable -- fresh marks mean the Castellan was here recently. Weathered marks mean the territory boundary is far or the Castellan has been displaced.
- Vocal: A deep, resonant call that carries through ruin corridors. The player will learn to use this call for distance and direction estimation.
**What makes it dangerous:** It knows the ruin terrain better than the player does. It uses verticality, blind corners, and narrow passages to ambush. It is fast in short bursts. Its attack is a single, high-damage pounce followed by a hold-and-bite.
**What makes it interesting:** It is predictable once understood. The 3-day patrol cycle, the territorial marks, the vocal calls -- all of these are readable. A player who invests time in learning a specific Castellan's patterns can move through its territory safely. Or can predict when the territory is unguarded and exploit the window.
**Relationship to ruins:** The Castellan is specifically adapted to the ruin environment. Its flexible body navigates rubble. Its claws grip concrete. It uses human infrastructure (collapsed floors, stairwells, elevator shafts) as ambush positions. It dens in enclosed spaces (parking garages, large interior rooms). It is the apex predator of human civilization's corpse.
**Visual archetype:** Angular, heavy. Dark coloration with lighter ventral surface. Eyes reflect light. Silhouette is immediately threatening -- low, wide, with visible mass in the shoulders and jaw.

#### 3.1.2 The Canopy Tyrant

**Ecological role:** Apex predator, deep forest / forest-ruin transition biome
**Primary biome:** Mature forest canopy and the tops of overgrown structures where tree canopy meets ruin canopy
**Body type:** Large arboreal predator. Long limbs, prehensile tail, powerful grip. Moves through the canopy with unsettling speed. Think a massive, predatory sloth-primate hybrid with none of the sloth's lethargy.
**Size:** 2.5-3 meters body length, plus 2-meter tail. Similar mass to the player creature but distributed for arboreal movement.
**Behavioral traits:**
- Semi-territorial. Defends a canopy zone but ranges widely when hunting.
- Active pursuit predator in three dimensions. Chases prey through canopy, across branches, over rooftops, through broken windows. Relentless in pursuit, capable of sustained speed that most prey cannot match in arboreal terrain.
- Crepuscular. Most active at dawn and dusk. Rests during midday (often visible draped across high branches, dangerously easy to mistake for a vine or branch if not alert).
- Vocalizes with a series of sharp, descending barks that echo through the canopy. The calls are used for spacing between individuals and are a reliable warning of presence.
- Builds crude nests from branches and scavenged material at the highest accessible points. Nests are visible from below and serve as territorial markers.
**What makes it dangerous:** The player creature is terrestrial-primary. The Canopy Tyrant controls the vertical space above the player. In forested areas and overgrown ruins with intact upper structures, the threat comes from above, which is difficult to monitor while navigating ground-level hazards. The creature is fast, persistent, and attacks from elevation advantage.
**What makes it interesting:** It creates a vertical threat axis that changes how the player uses the environment. Under canopy, the player must balance looking up (Canopy Tyrant) with looking around (ground threats). Entering the canopy (climbing) brings the player into the Tyrant's domain but away from ground predators. Traversal decision-making becomes richer.
**Relationship to ruins:** Uses the tallest surviving structures as nest sites and hunting perches. The transition zone where forest canopy meets ruin structure is its prime habitat. It has adapted to grip concrete and steel as well as branches. Window frames, balcony railings, and exposed rebar are all part of its arboreal highway.
**Visual archetype:** Long, angular, asymmetric. Limbs that seem too long for its body. Coloration that blends with bark and shadow -- mottled browns and greens. When still, it is nearly invisible in the canopy. When moving, its silhouette is unmistakable and alarming.

#### 3.1.3 The Deep King

**Ecological role:** Apex predator, underground biome
**Primary biome:** Flooded and dry underground spaces -- subway tunnels, deep basements, parking structures, utility corridors
**Body type:** Low-slung, elongated, eyeless or near-eyeless. Massive jaw structure. Smooth, pale skin. Sensory organs concentrated in specialized structures along the head and flanks (electroreception, vibration detection, chemoreception).
**Size:** 4-5 meters long. The largest terrestrial predator in the game. It can afford this size because the underground environment has no competition from other apex predators.
**Behavioral traits:**
- Solitary, highly territorial. One Deep King per major underground system.
- Detects prey through vibration and chemical sense. Completely silent when hunting. The player will not hear it approach. They may feel a subtle vibration in the controller (if haptics are supported) or see ripples in standing water.
- Ambush-dominant. Lies motionless in flooded areas or in debris for hours, then strikes with explosive speed. The attack window is less than a second.
- After feeding, retreats to a deep lair and is inactive for days. This creates exploitable windows.
- Does not patrol. Stations itself in high-traffic chokepoints (tunnel intersections, stairwell bases, flooded chambers) and waits. Location preference is semi-random, changing after each kill.
**What makes it dangerous:** The underground is the Deep King's world. Darkness, water, confined spaces, and the creature's ambush strategy combine to create a threat that is nearly invisible until the moment of attack. First encounters will likely be fatal. The creature teaches the player, through death, that the underground requires different rules.
**What makes it interesting:** It is beatable through understanding. Its vibration sense means the player can lure it with thrown objects. Its chemical sense means the player can mask their presence. Its post-feeding dormancy creates safe windows. Its chokepoint preference means the player can identify likely ambush sites. Every tool for defeating it comes from observation and understanding, not from stats or equipment.
**Relationship to ruins:** The underground infrastructure is its habitat. It navigates subway tunnels, moves through flooded parking garages, and squeezes through utility corridors that seem too small for its body. It has no interest in the surface world. It is specifically adapted to the artificial cave system that human construction created.
**Visual archetype:** Smooth, pale, elongated. Alien. No visible eyes. The jaw is the dominant feature -- wide, forward-facing, lined with recurved teeth. The body is almost serpentine but with vestigial limbs. It looks like something that evolved in the dark, which it did.

#### 3.1.4 The Glider

**Ecological role:** Apex predator, exposed highlands and open ruin fields
**Primary biome:** Open terrain -- collapsed areas with wide sightlines, elevated ridges, clear-cut zones around former airports or industrial parks
**Body type:** Large aerial predator. Broad wings (possibly membrane rather than feathered -- descended from a bat lineage that went large-body in the absence of human aviation). Powerful talons. Keen vision.
**Size:** 5-6 meter wingspan. Body mass comparable to the player creature.
**Behavioral traits:**
- Hunts from altitude. Soars on thermals, scanning for prey in open terrain below. Stoops (dives) at extreme speed for the kill.
- Diurnal. Does not hunt at night. Returns to a roost (cliff face, tall ruin structure) at dusk.
- Territorial in the air. Contests with other Gliders are aerial displays. Ground territory is not defended.
- Will not attack prey in dense cover (canopy, ruin interiors). Only targets creatures in the open.
- After a successful kill, feeds on the ground, during which it is vulnerable to terrestrial predators. This creates a potential opportunity for scavenging or even predation if the player is strong enough.
**What makes it dangerous:** In open terrain, the player is exposed to aerial attack from a predator that moves much faster and strikes from an angle the player cannot easily defend against. The stoop is nearly unavoidable in open ground.
**What makes it interesting:** It creates a cost-benefit calculation for open terrain. Open areas often offer faster travel and better sightlines -- but they expose the player to Glider attacks. This modifies route planning. The player learns to use cover, move along edges, and time crossings for when the Glider is elsewhere.
**Relationship to ruins:** Uses tall structures as roosts and launch points. The skeletal frameworks of collapsed high-rises provide ideal perching. Former communication towers and power line pylons serve as hunting platforms overlooking open terrain.
**Visual archetype:** Broad, angular wings. Dark against the sky. When soaring, it is a recognizable silhouette -- a warning sign the player learns to scan for.

### 3.2 Ambush Predators

#### 3.2.1 The Mossback

**Ecological role:** Ambush predator, overgrown ruin surfaces
**Primary biome:** Any area with dense surface vegetation on horizontal ruin surfaces (rooftops, collapsed floors, plazas)
**Body type:** Flat, wide, low-profile. Dorsal surface is textured and colonized by actual moss and lichen -- a living camouflage that is biological, not cosmetic. Resembles a section of overgrown concrete when stationary. Four short, powerful legs arranged for explosive lateral movement.
**Size:** 1.5-2 meters across, low profile (0.3 meters height when flattened). Roughly equal to the player creature in mass.
**Behavioral traits:**
- Absolutely stationary when hunting. Lies on vegetated surfaces and waits. Can remain motionless for hours.
- Strike trigger: Pressure on or immediately adjacent to its body. Prey (including the player) steps on it or close enough to it, and it attacks with a sudden lateral lunge and bite.
- After a failed strike, does not pursue. Returns to motionless ambush within seconds.
- Feeds where it catches. Remains on the carcass until finished, then relocates to a new ambush site.
- Not territorial. Multiple Mossbacks can share an area. Their spacing is determined by prey traffic, not social competition.
**What makes it dangerous:** It is invisible until the attack. The player will step on one and die before they know what happened. Repeated deaths in "safe" overgrown areas will teach the player that flat vegetated surfaces are not safe.
**What makes it interesting:** It is entirely avoidable once the player learns its camouflage pattern. Subtle visual tells (slightly different coloration from surrounding vegetation, a faint outline, the texture of its edges) differentiate it from actual terrain. The player develops a skill -- reading the ground -- that transfers across the entire game. Also, the Mossback can be exploited: the player can herd prey toward a known Mossback location and scavenge the kill.
**Relationship to ruins:** Evolved to exploit the flat, vegetated horizontal surfaces unique to ruin environments. Rooftops, plazas, wide corridors, and collapsed floors are its hunting ground. It has no equivalent in pre-human ecosystems because its niche (flat, hard substrate covered in thin vegetation) did not exist at this scale.
**Visual archetype:** Invisible until triggered. When attacking, reveals a wide, flat body with a disproportionately large mouth. The transition from "terrain" to "creature" is the visual shock.

#### 3.2.2 The Lurker

**Ecological role:** Ambush predator, aquatic/semi-aquatic zones
**Primary biome:** Flooded ruins, standing water in basements, flooded plazas, stagnant canals
**Body type:** Semi-aquatic. Crocodilian body plan adapted to artificial waterways. Long body, short powerful legs, laterally compressed tail for swimming. Eyes and nostrils positioned dorsally for surface ambush.
**Size:** 2-3 meters. Capable of taking prey up to and including the player creature.
**Behavioral traits:**
- Submerges in opaque water (common in flooded ruins where silt and organic matter cloud the water) with only eyes/nostrils exposed.
- Attacks prey at water's edge. Explosive lunge from water, grasps prey, drags underwater.
- Does not pursue on land. Strictly aquatic ambush. If prey escapes the initial lunge, the Lurker retreats underwater.
- Territorial over a specific body of water. One Lurker per small pool, 2-3 per larger flooded area.
- Basks on exposed surfaces during warm periods, which is the only time its full body is visible and the player can assess the threat.
**What makes it dangerous:** Water in flooded ruins is often unavoidable. The player must cross flooded areas, wade through partially submerged corridors, or navigate waterlogged terrain. The Lurker makes all water dangerous.
**What makes it interesting:** Water observation becomes a skill. Ripples, bubbles, subtle surface movement, the presence of basking Lurkers -- all readable signs. The player also learns that throwing objects into water can trigger a Lurker's strike, revealing its position before committing to crossing.
**Relationship to ruins:** The flooded infrastructure IS its habitat. Broken water mains, flooded basements, collapsed drainage, and artificial ponds created by subsidence are all Lurker territory. It hunts in spaces that humans built for other purposes entirely.
**Visual archetype:** Low, heavy, scaled. When submerged, nearly invisible. When lunging, explosive and violent -- the transition from still water to attack is one of the game's most visceral moments.

#### 3.2.3 The Weaver

**Ecological role:** Ambush predator, vertical and enclosed spaces
**Primary biome:** Interior ruin spaces with vertical access -- stairwells, elevator shafts, ventilation systems, narrow corridors
**Body type:** Arachnid-analog. Eight limbs arranged for grip on any surface. Compact body, long sensory palps, spinnerets producing adhesive silk. Not an actual spider -- a vertebrate that convergently evolved to fill the arachnid niche at a much larger scale.
**Size:** 0.8-1.2 meters body, 2-3 meter legspan. Smaller than the player creature but capable of restraining it.
**Behavioral traits:**
- Constructs adhesive web structures across passages, in shafts, and across doorways. The webs are visible but can be difficult to spot in dark interiors.
- Waits near its web. When prey contacts the web and is slowed/immobilized, the Weaver moves in quickly to bite. Venom is neurotoxic, causing progressive paralysis.
- Solitary and territorial over a specific structure or section of a building. One Weaver per floor of a building, roughly.
- Non-aggressive outside of web proximity. Will flee if encountered away from its web. The web is its weapon; without it, it is vulnerable.
- Rebuilds destroyed webs within hours. Destroying a web provides a temporary safe passage, not a permanent one.
**What makes it dangerous:** Interior ruin exploration requires entering Weaver territory. Dark corridors, narrow passages, and vertical shafts are all potential web sites. The webs are not always visible, especially in poor lighting. The venom creates a death spiral -- partial paralysis makes it harder to fight or flee, leading to further bites, leading to full paralysis.
**What makes it interesting:** Interior navigation becomes a puzzle. The player learns to scan for webs (looking for the characteristic sheen of adhesive silk), to carry or find objects to throw ahead and trigger webs, and to identify Weaver presence from secondary signs (silk remnants, wrapped prey carcasses, shed skin). The Weaver's webs also trap other creatures, so the player can sometimes find free food in a Weaver's larder -- but retrieving it means entering the web zone.
**Relationship to ruins:** Specifically adapted to the interior spaces of human buildings. Stairwells, which are architecturally universal in multi-story ruins, are its primary habitat. It has evolved in a niche that exists only because humans built enclosed vertical spaces.
**Visual archetype:** Spindly, angular, deeply unsettling. Long, thin limbs that grip walls and ceilings. Movements are quick and jerky. The body is compact and hard to hit. The visual design should trigger arachnophobia without being a literal giant spider -- something worse, because it is clearly a vertebrate that should not move this way.

#### 3.2.4 The Mimic

**Ecological role:** Ambush predator through aggressive mimicry
**Primary biome:** Ruin interiors, particularly areas with surviving human artifacts
**Body type:** Highly variable. The Mimic is a creature that has evolved to resemble commonly encountered inanimate objects in the ruin environment -- specifically, objects that attract scavenging creatures. Its body is flexible, capable of altering its outline, with chromatophore-like cells that allow limited color/texture shifting.
**Size:** Variable, 0.5-1.5 meters depending on what it is mimicking.
**Behavioral traits:**
- Adopts a stationary posture resembling a food source, water container, or shelter opening. It does NOT look exactly like a specific object -- it looks "close enough" to fool creatures that rely on shape recognition and color matching rather than detailed inspection.
- Waits for approach. When a creature comes within strike range (investigating the "food" or "water"), it unfolds and attacks.
- Relocates after each kill or failed attack. Does not use the same ambush posture in the same location twice.
- Solitary, non-territorial. Low energy requirements due to ambush efficiency.
**What makes it dangerous:** It exploits the player's learned behavior. The player learns that certain shapes and locations mean resources. The Mimic weaponizes that learning. It is the anti-pattern creature -- it punishes autopilot play.
**What makes it interesting:** Forces the player to inspect everything. Creates genuine paranoia in interior ruin exploration. Experienced players develop tells to distinguish Mimics from real objects (subtle movement, inconsistent details, behavioral reactions to thrown objects). The Mimic is a mastery test.
**Relationship to ruins:** Entirely ruin-dependent. Its mimicry targets are human artifacts. It could not exist without the specific visual environment of decaying human infrastructure. It is, in a sense, a predator that evolved to exploit human aesthetics.
**Visual archetype:** Unsettling. The moment of reveal -- when the "object" unfolds into a creature -- should be one of the game's most memorable visual moments. The body plan underneath the mimicry is bilateral, fleshy, and wrong-looking. Something that should not be able to fold into the shape it was just holding.

### 3.3 Prey Species

#### 3.3.1 The Grazer

**Ecological role:** Primary consumer, open and semi-open areas
**Primary biome:** Meadows in ruin clearings, overgrown parks, rooftop gardens, any area with dense ground-level vegetation
**Body type:** Medium ungulate. Compact, sure-footed, built for quick acceleration from a standing start. Comparable to a small deer adapted for broken terrain.
**Size:** 0.8-1.2 meters at shoulder. Smaller than the player creature.
**Behavioral traits:**
- Herding. Groups of 8-20 individuals. Safety in numbers and collective vigilance.
- Constant movement while feeding. The herd flows across the landscape, never staying in one location long enough for predators to set up.
- Sentinel behavior: 1-2 individuals maintain watch while others feed. Sentinels rotate. If a sentinel detects a threat, it produces an alarm call that triggers instant herd flight.
- Flight pattern: Explosive burst speed for 50-100 meters, then a course-change, then sustained moderate speed. This counters both ambush and pursuit predation strategies.
- Diurnal. Herds move to dense cover at dusk and shelter overnight. Night is too dangerous.
**What makes it food:** The most reliable food source for the player creature. Available in most surface biomes. Catchable with patience and strategy (isolating one from the herd, ambushing at a terrain chokepoint, intercepting during a flight course-change).
**What makes it interesting:** Herd dynamics create emergent gameplay. The player learns to read herd behavior -- the direction of sentinel attention, the herd's movement pattern, signs of stress (tight grouping, elevated heads) that indicate another predator is already nearby.
**Relationship to ruins:** Grazes on vegetation growing from ruin surfaces. Uses open ruin areas (plazas, parking lots reclaimed by grass, collapsed buildings providing meadow-like clearings) as primary feeding grounds. The ruins provide both food and the terrain complexity that makes the herd's evasion strategies effective.
**Visual archetype:** Rounded, smooth, earth-toned. Non-threatening silhouette. Large, dark eyes. The visual language communicates "prey" immediately.

#### 3.3.2 The Scrambler

**Ecological role:** Primary consumer, vertical/arboreal
**Primary biome:** Any terrain with significant verticality -- ruins with intact walls, cliff faces, large trees, overgrown building facades
**Body type:** Small, agile, equipped with adhesive pads and flexible digits for climbing any surface. Rodent-primate hybrid body plan.
**Size:** 0.3-0.5 meters body length. Much smaller than the player creature.
**Behavioral traits:**
- Colonial. Groups of 30-100 living in vertical terrain features (a ruined building face, a cliff, a large tree). The colony is a permanent structure; individuals may forage widely but return to the colony to rest.
- Primarily frugivorous/granivorous. Feeds on fruits, seeds, nuts, and new plant growth in the canopy and on vertical surfaces.
- Alarm system: The colony produces a cascading alarm call when a predator is detected. The call is species-specific -- the player will learn to identify what different alarm calls mean (aerial predator vs. terrestrial predator vs. climbing predator). This information is useful even though the Scrambler is reacting to a threat, not communicating with the player.
- Mobbing behavior: When a predator is detected near the colony, Scramblers swarm, vocalize loudly, and throw small objects. This does not cause significant harm but drives many predators away through persistent annoyance.
**What makes it food:** Individually, a very small meal. Catching them requires climbing skill and speed that the player creature may not initially have. They become a viable food source as the player creature develops arboreal capability.
**What makes it interesting:** The colony is an information system. Scrambler alarm calls provide real-time threat intelligence to the player who learns to decode them. A suddenly silent Scrambler colony means a predator has entered the area. A colony in full alarm indicates the threat's location and type. They are the player's early warning system, if the player pays attention.
**Relationship to ruins:** The vertical surfaces of human buildings are their primary habitat. Building facades with broken windows, exposed rebar, and crumbling ledges provide the grip-dense surfaces they need. They nest inside wall cavities, behind facades, and in ceiling spaces. They are the most common creatures the player will see in ruin environments.
**Visual archetype:** Small, rounded, quick. Large eyes for navigating dim interiors. Coloration varies by colony location (urban colonies are grayer; forest colonies are browner). Constantly in motion.

#### 3.3.3 The Rooter

**Ecological role:** Primary consumer / omnivore, ground-level
**Primary biome:** Forest floor, underground-adjacent areas, anywhere with deep leaf litter or loose substrate
**Body type:** Heavy, low, powerful. Built for digging and rooting through substrate. Pig-badger body plan. Tough, loose skin that resists bites and scratches. Short, powerful legs with heavy claws.
**Size:** 0.6-1 meter at shoulder. Comparable mass to the player creature due to dense build. Deceptively heavy.
**Behavioral traits:**
- Solitary or in mother-offspring groups. Not social beyond parental care.
- Omnivorous: roots, tubers, fungi, invertebrates, small vertebrates, carrion. Will eat almost anything. Spends most of its time nose-down, excavating.
- Aggressive when cornered or when offspring are threatened. Not a predator, but a capable fighter. The player creature may find that attacking a Rooter is harder than expected and not worth the injury risk versus the caloric reward.
- Creates significant environmental disturbance. Rooting behavior turns over soil, creates hollows, and exposes underground features. Areas with active Rooter populations have a churned, disturbed ground layer. This is ecologically important (soil aeration, seed dispersal) and visually distinctive.
- Does not flee easily. When threatened, faces the threat and displays aggression (lowered head, stomping, huffing). Only flees if significantly outmatched and given a clear escape route.
**What makes it food:** Decent caloric reward, but the Rooter fights back. Hunting Rooters is a risk-reward calculation: more food than a Scrambler, less dangerous than a Grazer herd (which requires dealing with herd vigilance), but the individual combat is non-trivial.
**What makes it interesting:** The Rooter's ground disturbance reveals things. Areas heavily worked by Rooters may have exposed underground passages, uncovered artifacts, or disturbed predator dens. Following a Rooter's trail can lead to discoveries. Their digging behavior is also audible, making them useful audio landmarks.
**Relationship to ruins:** Roots through the interface layer between ruin substrate and overgrowing soil. Regularly uncovers human artifacts, pieces of infrastructure, and buried surfaces. The Rooter's ecological role is, literally, excavation. It is the archaeologist of the animal world, though it has no interest in what it uncovers.
**Visual archetype:** Rounded but heavy. Dense, bristled appearance. Short limbs, heavy claws, broad snout. Visually reads as "sturdy" and "don't mess with me unless you're sure."

#### 3.3.4 The Darter

**Ecological role:** Primary consumer, aquatic/semi-aquatic
**Primary biome:** Clean water bodies -- flowing water in broken infrastructure, rainwater pools, clean-filtered flooded areas
**Body type:** Streamlined, semi-aquatic. Otter-fish hybrid. Equally capable in water and on wet surfaces. Fast swimmer, clumsy on dry land.
**Size:** 0.4-0.8 meters. Smaller than the player creature.
**Behavioral traits:**
- School-forming in water. Groups of 5-15.
- Feeds on aquatic vegetation, algae, invertebrates, and small fish.
- Extremely fast in water. Almost impossible for the player to catch while swimming. Catchable when they haul out onto dry surfaces (basking, nesting, transitioning between water bodies).
- Alarm behavior: The school dives and scatters when a threat is detected. In clear water, the visual effect of a school of Darters suddenly exploding into motion is distinctive and serves as a warning to the player that something (perhaps a Lurker) is present.
**What makes it food:** High caloric value relative to effort IF caught on land. In water, not worth pursuing.
**What makes it interesting:** Their presence indicates clean water, which may be important for the player. Their alarm behavior reveals Lurker positions. Their basking sites are predictable and become reliable hunting locations once identified. They connect the aquatic and terrestrial food webs.
**Relationship to ruins:** Uses broken infrastructure as habitat -- pipe systems as protected corridors, flooded basements as sheltered pools, broken fountains as basking platforms. Their presence maps the water flow of ruined infrastructure.
**Visual archetype:** Smooth, streamlined, iridescent. Quick, fluid movement. Pleasing to watch. The visual contrast between a school of Darters flowing through water and the decrepit ruin around them captures the game's aesthetic.

#### 3.3.5 The Dustwing

**Ecological role:** Primary consumer, aerial
**Primary biome:** Open air over all biomes. Roosts in high ruin structures and cliff faces.
**Body type:** Medium-sized flying creature. Feathered or membrane-winged. Highly maneuverable in flight.
**Size:** 0.5-1 meter wingspan. Small relative to the player creature.
**Behavioral traits:**
- Flock-forming. Groups of 20-50 in flight. Roosts communally in large colonies (hundreds to thousands) at specific locations.
- Frugivorous and insectivorous. Feeds in flight and by landing briefly on vegetation.
- Dawn and dusk mass flight from/to roost sites. The visual spectacle of thousands of Dustwings pouring from a ruin structure at dawn is a landmark event that orients the player -- roost sites are visible from great distances during mass flight.
- Individually non-threatening. Flock exhibits no defensive behavior beyond scattering.
**What makes it food:** Individually, barely a snack. Eggs (at roost sites) are a more reliable food source, but roost sites are often in dangerous, high locations guarded by the terrain itself.
**What makes it interesting:** Navigation aid. Dustwing roost sites are permanent, visible landmarks. Their mass flight patterns indicate wind direction and time of day. Their feeding flight paths indicate where fruit-bearing vegetation is concentrated. They are the world's information layer for observant players.
**Relationship to ruins:** Roost sites are exclusively in high ruin structures. The creatures are dependent on the vertical surfaces and enclosed cavities that human buildings provide. Without ruins, their roosting ecology would be limited to natural cliff faces.
**Visual archetype:** Delicate, flowing movement in groups. Individually small and unremarkable. In flocks, they create organic flowing patterns against the sky that are one of the game's signature visual moments.

#### 3.3.6 The Lumberer

**Ecological role:** Large primary consumer, megaherbivore
**Primary biome:** Deep forest and forest-ruin transition zones
**Body type:** Very large quadruped. Heavy, columnar legs. Long neck for reaching canopy vegetation. Elephant-giraffe body plan adapted to forest browsing.
**Size:** 4-5 meters at shoulder. The largest prey species and one of the largest creatures in the game. Significantly larger than the player creature.
**Behavioral traits:**
- Small groups (3-8). Matriarch-led. Slow, deliberate movement through the forest.
- Feeds on canopy vegetation, bark, and woody material. Its feeding activity creates forest clearings and modifies the canopy structure, which in turn creates habitat for other species.
- Non-aggressive but extremely dangerous if threatened. Its sheer mass makes any defensive action (stomping, swinging the neck, body-slamming) potentially lethal.
- Largely immune to predation as adults due to size. Calves are vulnerable and are actively defended by the group. A group defending a calf is one of the most dangerous situations in the game.
- Moves through the environment with notable impact -- broken branches, trampled ground, pushed-over small structures. Lumberer trails are visible and follow predictable routes.
**What makes it food:** An adult Lumberer is a feast that would sustain the player creature for days. But killing one is an extreme challenge, requiring understanding of its behavior, exploiting terrain, and sustained engagement. It is aspirational prey.
**What makes it interesting:** Its environmental impact creates opportunities. Lumberer trails provide clear paths through dense forest. Their feeding clearings create open areas where Grazers congregate (which the player can exploit). Their presence modifies the local ecosystem in visible ways. They are ecosystem engineers.
**Relationship to ruins:** Lumberers move through ruins as they move through forest -- by going through them. Walls, fences, and light structures are pushed aside. Only heavy concrete and steel resist them. Their trails through ruins are marked by structural damage that differs from natural decay -- fresher, directional, showing the force of passage.
**Visual archetype:** Massive, rounded, ancient-looking. Thick skin, possibly with moss or lichen growing on it (they move slowly enough to accumulate epiphytes). Visually reads as a geological feature that happens to move.

### 3.4 Scavengers

#### 3.4.1 The Picker

**Ecological role:** Obligate scavenger
**Primary biome:** Everywhere. Follows the food web's casualties.
**Body type:** Lean, long-legged, with a heavy beak/jaw adapted for tearing tough material. Vulture-hyena hybrid. Not built for killing but highly efficient at processing carcasses.
**Size:** 0.5-0.8 meters at shoulder. Smaller than the player creature.
**Behavioral traits:**
- Social. Packs of 5-15. Pack hierarchy determines feeding order at carcasses.
- Locates carcasses through keen sense of smell and by observing other scavengers and predator behavior from distance.
- Arrives at carcasses quickly after a kill. The appearance of Pickers overhead or converging on foot is a reliable indicator that a predator has made a kill nearby.
- Will scavenge from the player's kills, arriving within minutes. The player must choose: eat quickly, defend the carcass, or abandon it.
- Not aggressive to living creatures of the player's size or larger. Avoids confrontation. Will mob smaller creatures away from carcasses.
- Strips carcasses to bone efficiently. A Picker-processed carcass is a bare skeleton. This is how most skeletons the player encounters were made.
**What makes it useful:** Pickers are information. Their flight patterns indicate predator activity. Their convergence indicates a fresh carcass. Their absence from an area might indicate no predator activity (safe) or that a predator is so dominant that scavengers avoid the area (very dangerous).
**Relationship to ruins:** Roosts on high structures. Uses the vantage points that human buildings provide for spotting carcasses.
**Visual archetype:** Angular, gaunt, functional. Not beautiful. The efficiency of their design is their visual identity.

#### 3.4.2 The Crawler

**Ecological role:** Invertebrate decomposer collective
**Primary biome:** Everywhere, concentrated in warm, moist environments
**Body type:** Not a single creature but a colonial organism -- thousands of insect-like individuals that form a coherent mass when aggregated around a food source. Individually tiny. Collectively, a moving carpet of consumption.
**Size:** Individual: millimeters. Colony mass: up to several square meters of coverage on a carcass or organic deposit.
**Behavioral traits:**
- Aggregates at organic matter deposits (carcasses, fallen fruit, fungal blooms, waste).
- Moves as a coherent mass. The "flow" of a Crawler colony across a surface is a distinctive visual.
- Not aggressive, but a large colony can be hazardous to a creature that stumbles into it or rests on an active colony site. Individual bites are trivial; thousands of simultaneous bites cause distress and, eventually, harm.
- Activity is temperature-dependent. More active in warmth. Nearly dormant in cold. Server farm zones have massive Crawler populations due to waste heat.
- Chemically attracted to decay. The player creature, if injured, may attract Crawler attention. Open wounds draw them.
**What makes it interesting:** Crawlers are a constant, low-level environmental hazard that modifies behavior. The player learns not to rest near organic deposits. Injury management becomes important not just for health but for Crawler attraction. They are also a narrative device -- a carcass covered in Crawlers tells the player "something died here recently."
**Relationship to ruins:** Thrives in the warm, humid interiors of partially sealed ruins. Server farm zones are Crawler paradises. Their presence in a ruin indicates warmth and organic activity.
**Visual archetype:** Disturbing. The colony moves like a liquid. Individual creatures are too small to see in detail (mercifully). The visual effect is a living, flowing surface texture that replaces whatever it covers.

#### 3.4.3 The Bonecleaner

**Ecological role:** Specialized scavenger, processes the hardest organic materials
**Primary biome:** Bone deposit sites, former carcass locations
**Body type:** Medium-sized, with specialized jaws capable of crushing bone. Hyena-analog jaw structure. Stocky, powerful build.
**Size:** 0.6-0.9 meters at shoulder.
**Behavioral traits:**
- Follows Picker packs at a distance. Arrives at carcass sites after Pickers have stripped the soft tissue.
- Processes bone, cartilage, and dried tissue that other scavengers cannot exploit.
- Solitary or in mated pairs. Less social than Pickers.
- Caches bone fragments in hidden locations (dens, crevices). These bone caches can be found by the player and indicate Bonecleaner territory.
- Nocturnal. Processes bones at night, when Pickers are roosting and competition is minimal.
**What makes it interesting:** Bone caches are environmental landmarks. Bonecleaner activity indicates the local food web is active and functional. Their nocturnal activity means the player may hear them (the cracking sound of bone being crushed is distinctive and audible at moderate distance) without seeing them.
**Relationship to ruins:** Dens in enclosed ruin spaces -- basements, closets, under collapsed structures. Caches bones in these same spaces. The player may enter a ruin room and find a pile of bones -- not a horror set piece, but a Bonecleaner's pantry.
**Visual archetype:** Heavy jaw, muscular neck and shoulders tapering to a lighter rear body. Functional and unglamorous.

### 3.5 Territorial Defenders

#### 3.5.1 The Thornback

**Ecological role:** Territorial herbivore, aggressive defender
**Primary biome:** Dense vegetation zones -- thickets, overgrown gardens, vine-choked ruin interiors
**Body type:** Medium quadruped with dense, spine-covered dorsal and flank armor. The spines are modified quills -- not venomous, but sharp and barbed. Built like an armored boar.
**Size:** 0.8-1.2 meters at shoulder. Comparable to the player creature.
**Behavioral traits:**
- Fiercely territorial over specific vegetation patches (food source defense). Territory is small (20-50 meter radius) but aggressively defended.
- Warning display: Raises spines, stamps, snorts, and charges a short distance before pulling up. This is a genuine warning -- if the intruder (including the player) does not leave, the Thornback escalates to full contact.
- Charges and rolls when attacking, presenting the spine-covered back and flanks. Contact with the spines causes puncture wounds.
- Females with young are especially aggressive. A Thornback maternity territory is a no-go zone for anything that doesn't want a fight.
- Solitary male territories; females form small maternity groups.
**What makes it dangerous:** It controls access to desirable vegetation zones, which are often the same zones the player needs to traverse. The player must either fight through (costly), detour around (time-consuming), or learn the Thornback's behavioral cycle (it does leave the territory briefly to drink, and it sleeps during the hottest part of the day).
**What makes it interesting:** A predictable, solvable problem. The Thornback is a locked door with a behavioral key. Learning its patterns is a concrete, satisfying mastery challenge.
**Relationship to ruins:** Defends overgrown garden sites, greenhouse ruins, and areas with cultivated-descendant plant species. Its territory often inadvertently guards ruins that contain narrative content (a house whose garden the Thornback has claimed, blocking access to the interior).
**Visual archetype:** Rounded body, bristling with angular spines. The contrast between the round body (prey shape language) and the sharp spines (danger shape language) communicates "herbivore that will wreck you."

#### 3.5.2 The Resonant

**Ecological role:** Territorial omnivore, acoustic dominance
**Primary biome:** Enclosed ruin spaces with strong acoustic properties -- large halls, echoing corridors, domed structures, tunnels
**Body type:** Medium-large, with an overdeveloped vocal apparatus. Chest and throat contain resonating chambers capable of producing sounds ranging from subsonic rumble to focused high-frequency blasts. Otherwise a generalist omnivore body plan.
**Size:** 1-1.5 meters at shoulder. Larger than the player creature.
**Behavioral traits:**
- Territorial over acoustically significant spaces. A cathedral, a concert hall, a large parking garage -- any space where its vocalizations resonate effectively.
- Uses sound as its primary weapon. Territorial warning calls are subsonic rumbles that the player feels more than hears (controller vibration, screen distortion). Escalation produces focused sonic bursts that cause disorientation and, at close range, physical harm.
- Forages within its acoustic territory, eating a generalist diet of plants, fungi, and small animals.
- Solitary. Does not tolerate any large creature in its acoustic space. Two Resonants in proximity creates an escalating "shouting match" that can be heard from a great distance.
- Relatively timid outside its acoustic space. If driven from its territory (or encountered during rare outside forays), it is much less aggressive.
**What makes it dangerous:** Entering a Resonant's territory means entering a sound weapon zone. The acoustic attacks are difficult to dodge (sound fills spaces) and cause disorientation that makes navigation and combat harder. The creature exploits architecture for acoustic advantage.
**What makes it interesting:** A creature that weaponizes human architecture. The player learns which spaces are likely Resonant territories (large, enclosed, reverberant) and can prepare accordingly. The player can also exploit the Resonant's dependence on its space -- luring it outside its territory reduces its threat significantly.
**Relationship to ruins:** The most architecture-dependent creature in the game. It literally cannot function as a predator without human-built acoustic spaces. It is a species that could only evolve in a post-urban environment. Its territories mark the most architecturally significant surviving structures.
**Visual archetype:** Heavy chest, wide-set stance, oversized throat. When vocalizing, visible distortion of the air around its mouth (heat-haze effect). Not physically intimidating in silhouette -- the threat is audible, not visible.

### 3.6 Symbiotic/Mutualist Species

#### 3.6.1 The Lantern Bug

**Ecological role:** Bioluminescent insect, mutualist with multiple species
**Primary biome:** Dark environments -- underground, deep forest, interior ruins
**Body type:** Large insect (relative to insects). Soft body with bioluminescent organ. Emits a steady, warm glow.
**Size:** 3-5 centimeters. Tiny relative to the player creature.
**Behavioral traits:**
- Aggregates in swarms of 50-500. Swarm movement creates a moving light source in dark environments.
- Attracted to warmth and CO2 (exhaled breath of large animals). Will cluster around resting creatures, including the player creature, creating a bubble of illumination.
- Mutualistic relationship with several species: In exchange for warmth and CO2 proximity, the Lantern Bug's light attracts small insects, which creates a feeding opportunity for insectivorous creatures that tolerate the bugs' presence. The Lantern Bug also benefits from the larger creature's ability to deter its predators.
- Light output varies with environmental temperature. Brighter in cold. Dimmer in warmth. In server farm zones, they are nearly dark. In underground environments far from heat sources, they glow intensely.
**What makes it interesting:** Dynamic, emergent lighting system. The player creature attracts Lantern Bugs passively, gaining illumination in dark areas. But the light also reveals the player's position to predators. The illumination is useful but dangerous. The player must make decisions: attract bugs for light and accept the visibility risk, or move in darkness and accept the navigation risk.
**Relationship to ruins:** Thrives in dark interior spaces. Their aggregation in specific areas illuminates architectural features and artifacts that would otherwise be invisible, subtly guiding observant players toward narrative content without any explicit wayfinding system.
**Visual archetype:** Warm, golden-green light. Individual bugs are barely visible -- just points of light. In swarms, they create a flowing, organic illumination that transforms dark spaces into something ethereal.

#### 3.6.2 The Cleaner

**Ecological role:** Mutualist, ectoparasite removal
**Primary biome:** Open areas near water, "cleaning stations" at predictable locations
**Body type:** Small, brightly colored. Visible signal coloring that marks it as non-threatening. Dexterous manipulators for picking parasites from larger creatures' skin.
**Size:** 0.1-0.2 meters. Very small.
**Behavioral traits:**
- Establishes "cleaning stations" at fixed locations -- usually open, elevated areas near water.
- Larger creatures visit cleaning stations voluntarily. Even predators and prey species coexist temporarily at cleaning stations, bound by an evolved behavioral truce: no hunting at the cleaning station.
- Removes ectoparasites, dead skin, and wound debris from visiting creatures. This provides the Cleaner with food and the visitor with health benefits.
- The cleaning station truce is absolute for all species EXCEPT those that have not learned it. The player creature must learn (through observation) that cleaning stations are safe zones and that visiting one provides tangible benefit (parasite removal, wound cleaning, possibly a small health regeneration effect).
**What makes it interesting:** Cleaning stations are social hubs of the animal world. The player can observe predator and prey species in proximity without conflict. Behavioral information is dense at these locations -- the player sees creatures out of their normal behavioral context, relaxed and tolerant. The truce mechanic introduces a rule about the world that the player must discover and can choose to respect or violate (violating it has consequences -- other creatures will not visit a cleaning station where a predator has broken the truce, and the player loses access to the cleaning benefit).
**Relationship to ruins:** Cleaning stations are often located on flat, exposed ruin surfaces -- plaza edges, bridge abutments, low walls. The Cleaner has adapted to human-created landscape features as its cleaning platform.
**Visual archetype:** Bright, conspicuous, small. The visual opposite of camouflaged predators. Their coloration signals safety, which is a visual language the player learns to read.

#### 3.6.3 The Gardener

**Ecological role:** Mutualist with plant species, seed disperser and cultivator
**Primary biome:** Forest edges, overgrown gardens, ruin perimeters
**Body type:** Medium-sized, semi-arboreal. Cheek pouches or specialized gut for seed transport. Dexterous forelimbs for digging and planting.
**Size:** 0.3-0.5 meters. Smaller than the player creature.
**Behavioral traits:**
- Frugivorous. Eats fruit and disperses seeds. But unlike passive seed dispersal, the Gardener actively caches seeds in favorable growing locations -- creating intentional "gardens" that it returns to harvest when the plants mature.
- Maintains multiple garden sites within its home range. Visits them regularly to tend (removing competing vegetation, loosening soil) and to harvest.
- Territorial over garden sites, but non-aggressive. Defends gardens through alarm calls and by fleeing while calling, which attracts other creatures' attention to the intruder.
- Its gardening behavior is visibly different from random animal activity. The player can observe the Gardener digging, placing seeds, and returning to specific locations -- behavior that echoes human agriculture. This is convergent evolution, not inherited behavior, but the visual parallel is striking in a game about the loss of human cognition.
**What makes it interesting:** The Gardener's garden sites are reliable food sources for the player, if the player can find them. They are also ecologically important -- Gardener activity drives plant community composition in an area. And thematically, the Gardener is doing what humans did, on a smaller scale, without consciousness of the parallel. The player may or may not notice this.
**Relationship to ruins:** Gardens are often located in former human garden sites -- planter boxes, courtyards, window boxes -- where the soil conditions are favorable due to generations of human cultivation. The Gardener has inherited human garden spaces without inheriting human gardening.
**Visual archetype:** Busy, purposeful, endearing. Rounded body, bright eyes, always carrying something. The most "likeable" creature design, which makes it thematically useful as a contrast to the predatory world around it.

---

## 4. The Player Creature

### 4.1 Design Philosophy

The player creature is not a hero, a chosen one, or a special specimen. It is a wild animal. It exists within the food web, subject to the same rules as every other creature. Its capabilities are appropriate to its ecological niche. It does not have abilities that violate the biological logic of the world.

The player creature is special only in the sense that the player inhabits it -- and the player brings human consciousness to bear on a world that no longer contains any. This is the central irony: the player is, in a sense, reintroducing human-level cognition to a world that lost it. But only from outside the screen.

### 4.2 Starting Form and Capabilities

**Body plan:** Medium-sized generalist predator/omnivore. Think of a marten, wolverine, or small bear -- a creature built for adaptability rather than specialization. Four limbs, capable of both quadrupedal and semi-bipedal movement. Flexible spine, good grip, decent speed, moderate endurance.

**Starting capabilities:**
- Terrestrial locomotion (running, jumping, climbing short obstacles)
- Basic combat (bite, claw swipe -- sufficient for prey its size or smaller, insufficient for apex predators or large prey)
- Omnivorous diet (can eat plant material, fungi, insects, small prey, carrion)
- Moderate sensory capability (sight, hearing, smell -- represented through visual/audio design, not HUD elements)
- Survival instincts (behavioral animations indicating danger, hunger, fatigue -- the creature communicates its state to the player through body language, not numbers)

**What it CANNOT do at start:**
- Swim effectively (shallow wading only)
- Climb smooth vertical surfaces
- Survive combat with any apex predator
- Navigate total darkness (no echolocation or night vision)
- Sustain extended activity without rest and food

### 4.3 Food Web Position

The player creature begins as a secondary consumer -- a mesopredator. It eats prey species (Grazers, Scramblers, Darters) and competes with other mesopredators. It is prey to all apex predators and most ambush predators.

This position is narratively and mechanically appropriate:
- It is not at the bottom (the player would die constantly with no agency)
- It is not at the top (there would be no meaningful threat)
- It is in the middle, where every encounter requires assessment: Am I the predator or the prey in this situation?

As the player develops mastery (through knowledge, not through leveling), the creature's effective position in the food web can shift. A player who understands Castellan patrol patterns, Mossback camouflage tells, and Weaver web locations can navigate the world as if they were above these threats -- not because the creature has gotten stronger, but because the player has gotten smarter.

### 4.4 Why It's Not Special (and Why That Matters)

The player creature has no unique biological advantage. It is not the only one of its kind. Other creatures of the same species exist in the world (and may be encountered, particularly during breeding season). It is not faster, stronger, tougher, or smarter (in animal terms) than other creatures in its ecological niche.

This matters because:
- The world's indifference is credible only if the player creature is not exempt from it
- The mastery theme requires that the player's advantage comes from the human at the screen, not from the avatar on screen
- Encountering other members of the player creature's species reinforces its ordinariness and grounds it in the ecology
- The creature's death is not narratively significant within the game world -- it is one animal dying, as animals do. This creates appropriate stakes without melodrama.

### 4.5 Behavioral Tells

Since there is no HUD, the creature communicates its internal state to the player entirely through animation, sound, and behavioral change. This is the primary design challenge of the player creature.

**Hunger states:**
- Satiated: Relaxed posture, smooth movement, occasional idle behaviors (grooming, sniffing curiously at non-food objects)
- Hungry: More alert, head tracks potential food sources, movement becomes more directed and purposeful
- Starving: Visible physical change (sides drawn in, movement less fluid), behavioral desperation (attempting to eat things it normally wouldn't, reduced flight response to threats because the energy cost of fleeing exceeds the risk of staying)

**Fear states:**
- Calm: Upright posture, ears forward, tail relaxed
- Alert: Head up, ears swiveling, freeze-and-scan behavior
- Alarmed: Low posture, ears back, ready-to-bolt tension visible in the legs
- Panicked: Full flight. The creature runs and the player's control is partially reduced -- the creature will avoid running toward the threat even if the player tries to direct it there. This is the creature's survival instinct overriding the player's commands, which communicates "you are in real danger" without any text or icon.

**Fatigue states:**
- Rested: Full range of movement, maximum speed and jump capability
- Tired: Slightly slower, occasional stumbles on rough terrain, heavier breathing audio
- Exhausted: Significantly impaired movement, the creature will resist the player's input to continue running and will seek rest locations autonomously if the player does not direct it to rest

**Injury states:**
- Healthy: Normal movement
- Wounded: Favoring the injured limb/area. Movement asymmetry. Visible wound. Occasional pain vocalization. Crawlers begin to track the creature if the wound is open.
- Critical: Severely impaired. Limping, dragging, labored breathing. The world looks different (color desaturation, narrowed focus -- visual effects representing the creature's altered perception, not gamified "low health" filters).

**Environmental response:**
- Cold: Shivering, fur/skin puffing, seeking warmth
- Heat: Panting, seeking shade, reduced activity
- Wet: Shaking off water, discomfort on saturated surfaces
- Darkness: Wider eyes, slower movement, visible tension, ears working overtime

---

## 5. The Devolved Humans

### 5.1 Design Philosophy: Respect

The devolved humans are the most narratively sensitive creatures in the game. They must be designed with absolute respect. They are not zombies, not monsters, not enemies, not comedy, not body horror. They are animals -- hominid animals whose ancestors were people.

The design goal is the uncanny valley of recognition. The player (the human at the screen) should look at these creatures and feel a slow, dawning discomfort. Not because the creatures are scary, but because they are familiar. The body plan is human. The face is human. The behavior is not.

This discomfort IS the narrative payload. The devolved humans do not need text or lore entries to tell their story. They tell it by existing.

### 5.2 Physical Design

**Body plan:** Anatomically modern human. No mutations, no deformities, no monstrous features. They look like people. Proportions are human. Skin, hair, facial features are human.

**Differences from modern humans (subtle, accumulated over centuries of changed selection pressures):**
- Slightly heavier build (life is more physical now)
- Tougher skin, thicker calluses (no clothing, no shoes)
- Body hair may be slightly denser (thermoregulation without clothing)
- Dental wear patterns consistent with a rough, unprocessed diet
- Musculature is functional, not atrophied -- these are active, foraging animals

**What is preserved:** The face. Human faces are the most recognizable thing in the world to human observers. The devolved humans have human faces. They have expressions. They smile (a social affiliative signal), frown (discomfort), bare teeth (threat), widen eyes (fear). These expressions are readable by the player because human facial expression recognition is deeply hardwired.

**What is gone:** The eyes. Not physically -- the eyes are anatomically normal. But there is no "spark" of higher cognition visible in them. No curiosity about abstract things. No contemplation. No recognition of the player creature as anything other than a potential threat or non-threat. The eyes are watchful, alert, emotionally present, but cognitively absent. This is the uncanny valley.

### 5.3 Behavioral Patterns

The devolved humans behave like social omnivorous primates, which is what they are.

**Daily routine:**
- Wake at dawn in communal sleeping site (enclosed spaces in ruins -- rooms, overhangs, cave-like areas)
- Morning social grooming (10-15 minutes of reciprocal grooming, pair-bonding maintenance)
- Foraging group forms and moves out. Females with young stay closer to the sleeping site. Adult males and non-parenting females range further.
- Foraging is continuous throughout daylight hours. Diet: fruits, roots, nuts, insects, small animals, eggs, carrion. Seasonal variation based on availability.
- Midday rest during heat (sheltering in shade, reduced activity)
- Afternoon foraging, often at different locations than morning (resource rotation, whether intentional or habitual)
- Return to sleeping site before dusk
- Evening social activity (vocalizing, grooming, play among juveniles)
- Sleep

**Social structure:**
- Bands of 15-40 individuals
- Matrilineal organization (females are the stable core; males transfer between bands)
- Dominance hierarchy maintained through display and occasional physical contest (rarely injurious)
- Affiliative behavior is prominent: grooming, food sharing, proximity-seeking, cooperative vigilance
- Juveniles play: chasing, wrestling, object manipulation. The play is similar to what you'd see in chimpanzee or bonobo juveniles. It is not symbolic play (no pretending, no narrative games). It is physical and social.

**Foraging behavior:**
- Simple tool use: Using rocks to crack nuts. Using sticks to extract insects from crevices. Using leaves as scoops for water. Single-step, observable-cause-and-effect tool use. No manufacture (no shaping a tool before use).
- Memory-based foraging: The band returns to known food sources seasonally. This requires spatial memory but not planning.
- No cooking. No food storage. No food processing beyond immediate mechanical processing (cracking, peeling, stripping).

**Threat response:**
- To the player creature: Assessed based on size and behavior. The player creature is roughly mesopredator-sized, so the devolved humans will be wary but not panicked. If the player creature does not approach aggressively, the humans will maintain distance and continue their activities. If the player creature approaches, the nearest adults will display (standing tall, vocalizing, possibly brandishing found objects). If the player creature attacks, the band will flee while adult males attempt to delay the threat.
- To apex predators: Immediate, coordinated flight. The band has learned (culturally, transmitted through behavior modeling) which threats require flight. Alarm calls are specific to threat types (aerial vs. terrestrial).
- To other human bands: Typically avoidance. Bands maintain spacing. Encounters at resource sites can produce tension (display, vocalization) but rarely escalate to inter-band violence.

### 5.4 The Uncanny Valley of Recognition

The narrative power of the devolved humans comes from specific moments of recognition -- moments where the player sees something human in animal behavior and is struck by the gap.

**Designed recognition moments (not scripted, but emergent from the behavioral system):**
- A mother holding an infant to her chest, stroking its hair, vocalizing softly. Universally recognizable maternal behavior. The mother's face shows tenderness. The scene is indistinguishable from a human mother comforting a child -- until the mother picks up a grub with her free hand and eats it without interrupting the vocalization, and the infant reaches for the grub with a hand that has never held a tool, and the moment is simultaneously human and animal.
- An individual picking up a found object (a piece of corroded metal, a colored stone, a shard of glass) and examining it. Turning it over. Holding it up to the light. Then discarding it. The examination is curiosity, but it is object-curiosity (does it smell interesting? is it edible? is it a threat?) not abstract-curiosity (what is this? who made this? what does this mean?).
- An individual standing at the edge of a high ruin structure, looking out over the landscape. The silhouette is human. The posture is human. For a moment, the player might project contemplation, reflection, appreciation of beauty. Then the individual urinates off the edge and returns to foraging, and the projection collapses.
- Juveniles playing near a collapsed school. Running, chasing, vocalizing with joy. Playing in a schoolyard, as children do. The game does not underline the irony. The irony underlines itself.

### 5.5 Ecological Niche

In the food web, the devolved humans occupy a specific niche:

- **Trophic level:** Omnivore, secondary consumer. Same level as the player creature.
- **Competitors:** Other medium-sized omnivores, including the player creature's species.
- **Predators:** All apex predators. Ambush predators depending on context. Devolved humans are vulnerable to the same threats as any similarly-sized animal.
- **Advantages:** Social cooperation (group vigilance, group defense), moderate tool use, manual dexterity (they can access food sources that require manipulation -- cracking, peeling, extracting), behavioral flexibility (generalist diet and habitat use).
- **Disadvantages:** No technology, no sustained planning, no environmental modification beyond incidental damage.

The devolved humans are successful animals. Their population is stable. They are not going extinct. They have found a niche and they fill it effectively. This is important: their story is not ongoing tragedy. It is completed transition. They are what they are, and what they are is viable.

### 5.6 Narrative Impact

The devolved humans deliver narrative impact through three mechanisms:

1. **Recognition:** The player recognizes the human body plan and is drawn to observe. Every observation reveals the gap between appearance (human) and behavior (animal). This gap is the narrative.

2. **Contextualization:** Encountering devolved humans AFTER discovering narrative artifacts (research papers, children's drawings, Dr. Osei's journals) transforms the encounter. The player now knows what these creatures used to be. The same behavioral observation that was merely interesting before becomes devastating.

3. **Reflection:** The devolved humans force the player to confront the game's thematic questions directly. Are these creatures suffering? Are they conscious? Are they "less than" their ancestors or simply "different from"? Does the player have the right to judge? The game provides no answers. The devolved humans just ARE.

---

## 6. Creature Interaction Matrix

### 6.1 Predator-Prey Pairings

| Predator | Primary Prey | Secondary Prey | Notes |
|---|---|---|---|
| Castellan | Grazer, Rooter | Devolved Humans, Player Creature, Thornback | Primary urban apex; hunts via ambush in ruins |
| Canopy Tyrant | Scrambler, Dustwing | Grazer (canopy edge), Player Creature | Arboreal specialist; rarely descends to ground |
| Deep King | Darter, Rooter (underground) | Player Creature, anything in tunnels | Underground exclusive; indiscriminate ambush |
| Glider | Grazer, Dustwing | Player Creature, Devolved Humans | Aerial apex; only attacks in open terrain |
| Mossback | Grazer, Rooter, Scrambler | Player Creature | Ambush on vegetated surfaces; non-selective |
| Lurker | Darter, Grazer (at water) | Player Creature, Devolved Humans | Aquatic ambush at water's edge |
| Weaver | Scrambler, Dustwing | Player Creature | Web-based; size-limited prey |
| Mimic | Scrambler, Rooter | Player Creature | Ambush via mimicry; targets scavengers |
| Player Creature | Grazer, Scrambler, Darter | Rooter, Dustwing eggs, insects | Generalist mesopredator |

### 6.2 Competitive Relationships

**Direct competition (same food resources):**
- Player Creature vs. Devolved Humans: Compete for fruit, roots, eggs, small prey, and carrion. Neither species is dominant; outcomes depend on group size (humans have numerical advantage) and individual capability.
- Player Creature vs. Rooter: Compete for ground-level food. The Rooter accesses subterranean food the player cannot. The player accesses arboreal food the Rooter cannot. Overlap is in surface-level resources.
- Thornback vs. Grazer: Compete for vegetation. The Thornback's territorial aggression creates grazer-exclusion zones, pushing Grazer herds to other areas.
- Picker vs. Player Creature: Compete for carrion. Pickers arrive quickly and in numbers. The player must eat fast or defend.

**Interference competition (space and territory):**
- Castellan vs. Canopy Tyrant: At the boundary between urban and forest biomes, these two apex predators contest the transition zone. Neither is dominant in the other's primary habitat. Encounters are display-based and rarely escalate to combat.
- Resonant vs. all species: The Resonant excludes everything from its acoustic territory. This creates predator-free zones that prey species exploit (grazing near but not in a Resonant's territory, knowing the Castellan will not enter).
- Devolved Humans vs. other band-forming species: Competition for sleeping sites and shelter resources. Devolved humans preferentially occupy enclosed ruin spaces, which displaces other species that might use them.

### 6.3 Symbiotic Relationships

**Mutualism:**
- Lantern Bug + multiple species: Light-for-warmth mutualism in underground environments. The player creature benefits. So do other creatures.
- Cleaner + all large species: Parasite removal mutualism at cleaning stations. The cleaning station truce is a cross-species behavioral norm.
- Gardener + fruiting plants: Seed dispersal and cultivation mutualism. The Gardener's activity increases local fruit production, benefiting all frugivores.
- Scrambler alarm system + all prey species: Scrambler alarm calls benefit all species that can hear them. This is not true mutualism (the Scramblers don't benefit from other species' presence) but is a form of information parasitism that benefits the broader prey community.

**Commensalism:**
- Picker + apex predators: Pickers benefit from predator kills without affecting predator fitness. The relationship is one-directional.
- Dustwing roosts + building-dwelling species: Dustwing guano enriches soil at roost bases, supporting plant growth that feeds other herbivores. The Dustwings are unaffected.
- Lumberer trail-following: Multiple species use Lumberer trails as movement corridors. The Lumberer is unaffected by the followers.

**Parasitism:**
- Crawler + injured creatures: Crawlers are attracted to wounds and can impede healing. They benefit; the host is harmed.
- Various ectoparasites (not individually modeled but represented through the Cleaner mechanic): Parasites reduce creature fitness; Cleaner mutualism is the counterbalance.

### 6.4 How the Player Disrupts These Relationships

The player is not a passive observer. The player's actions have ecological consequences:

**Predator removal:**
- Killing or driving off an apex predator releases mesopredator populations in that territory. More mesopredators means more competition for the player. More prey consumption by mesopredators means potential prey population decline. The benefit (removing a major threat) has costs (ecological disruption).
- This is not a morality system. It is ecology. The player may or may not notice the downstream effects.

**Prey depletion:**
- Over-hunting a prey species in an area reduces food availability for all predators in the area, including the player. Predators may shift to alternative prey (including the player creature or devolved humans) or leave the area.
- Prey depletion also reduces Picker and scavenger activity, which reduces the information value of scavenger behavior (fewer kills to converge on).

**Territorial disruption:**
- Killing a Thornback opens its territory to grazers, which changes local vegetation patterns, which alters the landscape over time.
- Killing a Resonant opens its acoustic space to other species, which changes the local threat landscape.

**Resource competition:**
- Eating fruit that a Gardener has been cultivating depletes the Gardener's food source and may cause it to relocate, losing the player a reliable future food source.
- Scavenging at a Bonecleaner's cache deprives it of stored food, potentially driving it to more visible scavenging behavior, which attracts predators.

**Cleaning station behavior:**
- Attacking prey at a cleaning station breaks the truce. Other species stop visiting. The cleaning station ceases to function. The player loses a health benefit, a behavioral observation site, and a safe zone.
- This is the most explicit behavioral-consequence system in the game. It teaches the player that the world's rules have value and that breaking them has costs.

### 6.5 Seasonal and Temporal Dynamics

The interaction matrix is not static. It shifts with:

**Time of day:**
- Diurnal predators (Glider, Canopy Tyrant) are active during the day. Nocturnal creatures (Bonecleaner, some Crawlers) are active at night. Crepuscular species (Canopy Tyrant at the edges of its range) bridge the transitions.
- The threat landscape changes completely between day and night. A location that is safe during the day (under Glider patrol, which the player can see and avoid) becomes dangerous at night (nocturnal predators the player cannot see).

**Season:**
- Breeding season increases aggression in territorial species and creates nest-defense encounters.
- Migration season shifts prey distribution, pulling predators along with their prey.
- Wet/dry seasons affect water levels in flooded ruins, opening or closing access to areas and changing Lurker distribution.
- Fruit seasons affect Gardener, Scrambler, and Dustwing behavior and distribution.

**Weather:**
- Rain reduces visibility and muffles sound, favoring ambush predators and disadvantaging pursuit predators.
- Wind affects Glider hunting capability (too much wind prevents precision stooping) and Dustwing flight patterns.
- Temperature extremes change Crawler activity, Lantern Bug brightness, and creature activity budgets generally.

### 6.6 Emergent Interaction Scenarios

The interaction matrix is designed to produce emergent scenarios that surprise both the player and the designers:

- A Castellan kills a Grazer. Pickers converge. The player arrives to scavenge. A second Castellan arrives (territorial dispute -- the kill happened near a boundary). The two Castellans display at each other while the Pickers scatter and the player has a narrow window to grab food from the carcass while both predators are focused on each other.

- A Lumberer herd moves through a Thornback's territory. The Thornback displays but cannot drive off a creature fifty times its mass. The Lumberers pass through, destroying the Thornback's defended vegetation in the process. The Thornback's territory is effectively erased. It must relocate, which changes the local threat landscape for the player.

- Devolved humans and the player creature converge on the same food source. Neither is aggressive. Both feed warily, maintaining distance. A Castellan arrives. Both species flee. In the chaos, the player creature runs in the same direction as the human band and is temporarily mixed in among them. The humans are alarmed by the player creature's proximity but more alarmed by the Castellan. For a few seconds, they are fleeing together, two species united by shared predation pressure. Then the Castellan selects a target (one of the slower humans, a straggler) and the player creature is free. The player watched someone die who looked almost human. The game says nothing about it.

- A Mossback catches a Darter near a cleaning station. The kill disrupts the cleaning station's truce zone. Creatures that were peacefully receiving cleaning scatter. The cleaning station goes silent for a day. The Mossback, not a social learner, does not understand the truce it broke. The ecosystem recovers the station eventually, but the player's visit during the disrupted period finds an empty, useless cleaning station, and they may never know why.

These scenarios are not designed as set pieces. They emerge from the interaction of behavioral systems. The game is richer for their unpredictability.
