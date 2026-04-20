# Audio Design Document

## 1. Audio Philosophy

Rewild has no soundtrack. The world is the soundtrack.

Traditional game audio treats music as emotional scaffolding -- telling the player what to feel, when to feel tense, when to feel triumphant. Rewild rejects this. The player exists inside an ecosystem that does not perform for them. The soundscape is a living system, and the player's survival depends on learning to read it.

This directly serves **Pillar 2: Mastery Through Understanding**. Sound is information. A player who learns to parse the audio layer gains a genuine survival advantage -- identifying predators before visual contact, locating water sources by ear, reading the behavioral state of nearby creatures from their vocalizations. The audio system rewards attentive listening the same way the visual system rewards careful observation.

### Core Principles

- **No traditional music.** The ecosystem generates the sonic environment. There is no composed score, no dynamic music system reacting to gameplay state. The world sounds the way it sounds because of what is happening in it.
- **Rare exceptions for discovery moments.** When the player finds a narrative fragment deep inside a ruin -- a piece of the old world's story -- a single sustained tone emerges. Not a melody. A resonance. Like a memory surfacing. These moments are the only departure from pure diegetic audio, and their rarity makes them powerful.
- **Sound as survival tool.** Every sound in the game carries information. Rain tells you about terrain topology. Creature calls tell you about predator proximity. Silence tells you something has gone wrong.
- **Spatial audio is primary.** The player should be able to close their eyes and know what surrounds them. Stereo positioning, distance attenuation, and occlusion are not polish features -- they are core gameplay systems.

---

## 2. Ambient Soundscape Architecture

### Per-Biome Sonic Identity

Each biome has a unique base ambience that functions as its auditory fingerprint. The player should be able to identify their biome by sound alone.

| Biome | Base Atmosphere | Signature Sound |
|-------|----------------|-----------------|
| Overgrown City | Wind through broken buildings, distant water drip, settling concrete | Deep structural groans of decaying skyscrapers |
| Reclaimed Forest | Dense insect layer, canopy rustle, filtered wind | Overlapping birdsong at varying distances |
| Flooded District | Lapping water, submerged echoes, dripping | Hollow resonance of half-submerged structures |
| Root Network (Underground) | Near-silence, dripping, organic creaking | Bass rumble of root systems shifting |
| Server Farm Ruins | Electrical hum, fan whir, data noise | The uncanny drone of machines running without purpose |
| Coastal Erosion Zone | Waves, wind, salt-crusted metal stress | Rhythmic crash patterns that shift with weather |

### Layered Mixing System

The soundscape is built from four simultaneous layers, each independently controlled:

1. **Base Atmosphere Layer** -- Always present. The biome's foundational sound. Looping ambient beds with long crossfades to avoid perceptible repetition. Minimum 3-minute loop lengths, with randomized variation layers on top.

2. **Creature Layer** -- Populated by the AI simulation. Each creature within audible range contributes its vocalization to the mix. This layer is genuinely dynamic -- it reflects the actual creature population and their behavioral states. When predators clear an area, the creature layer thins. When an ecosystem is thriving, it's dense and layered.

3. **Weather Layer** -- Driven by the weather state machine. Rain, wind, and storm audio with smooth transitions between intensity levels. Weather audio interacts with environment geometry -- rain sounds different under a canopy than in the open, different inside a ruin than outside.

4. **Ruin/Structure Layer** -- Active near human-built structures. Creaking, settling, dripping, electrical hum. Intensity scales with the ruin's decay state. Freshly-collapsed structures are louder and more active than ancient, settled ones.

### Dynamic Mixing States

The relative balance of layers shifts based on the player's situation:

- **Exploration** -- All layers at natural levels. The full ecosystem is audible. The creature layer is prominent.
- **Tension** -- Creature layer begins to thin (prey animals go quiet). Base atmosphere becomes more prominent. A subtle low-frequency presence enters the mix.
- **Pursuit** -- Creature layer drops to near-silence. Weather layer attenuates. The mix narrows to essentials: the player's own sounds (breathing, footsteps), the predator's sounds (movement, vocalizations), and spatial cues for navigation.
- **Shelter** -- Outside sounds are muffled by geometry occlusion. Interior ambience takes over. The player's breathing slows. If raining, the sound of rain on the shelter's surfaces creates a cocoon effect.
- **Discovery** -- When entering a narrative space, layers fade to leave room for the discovery tone. The transition should feel like the world holding its breath.

### Day/Night Audio Shifts

The creature layer transitions naturally between diurnal and nocturnal populations:

- **Dawn** -- Gradual bird/insect onset. Nocturnal creatures fade. A brief period of peak density where both populations overlap.
- **Day** -- Full diurnal creature layer. Higher frequency bias. More activity, more variety.
- **Dusk** -- Diurnal creatures wind down. Brief quiet transition. Nocturnal creatures begin.
- **Night** -- Nocturnal creature layer. Lower frequency bias. Fewer species, but more spatially distinct. Individual sounds carry further in the quieter night mix.

### Distance-Based Detail

Audio detail scales with proximity:

- **Close (0-3 tiles)** -- Individual, high-detail sounds. You can hear a specific creature's footsteps, the particular pattern of dripping from a specific pipe.
- **Mid (3-10 tiles)** -- Sounds blend into clusters. Individual creatures are less distinct. You hear "a group of creatures" rather than specific individuals.
- **Far (10+ tiles)** -- Pure texture. Creature sounds become a wash. Weather dominates. You get a sense of the distant biome's character without individual detail.

This gradient is critical for spatial awareness. The player should feel the world extending beyond their viewport in every direction.

---

## 3. Creature Audio Design

### Vocal Signature System

Every creature species in Rewild has a unique vocal signature -- a set of sounds that identifies it as clearly as its visual silhouette. The design goal: a skilled player can identify any creature by sound alone and know what it's doing.

### Behavioral State Vocalizations

Each species has distinct vocalizations for each behavioral state:

| Behavioral State | Audio Character | Player Information |
|-----------------|----------------|-------------------|
| **Idle/Resting** | Soft, rhythmic, low energy | Safe zone indicator. This creature is relaxed. |
| **Foraging** | Intermittent, varied pitch, movement sounds | Normal ecosystem activity. No threat. |
| **Alert** | Sharp, clipped, elevated pitch | Something has changed. Possible incoming threat. |
| **Territorial** | Loud, repetitive, aggressive tone | Stay out. Boundary warning. |
| **Hunting** | Minimal vocalization, movement sounds only | Active predator. Silence is the warning. |
| **Distress** | High-pitched, rapid, irregular | A creature is being attacked. Predator nearby. |
| **Mating** | Patterned, musical, species-specific | Seasonal indicator. Safe period for that species. |
| **Pack Communication** | Call-and-response patterns | Coordinated behavior. Multiple threats or allies. |

### Predator Audio Design

Predator audio is designed to trigger genuine unease:

- **Low frequency dominance.** Predator vocalizations sit in the 60-200Hz range. This is felt as much as heard. The player's subconscious registers predator presence before conscious identification.
- **Directional precision.** Predator audio has tight stereo positioning. The player should be able to localize a predator's direction from sound alone.
- **Minimal vocalization during hunt.** A hunting predator goes quiet. The player learns that the absence of predator calls is more dangerous than their presence -- a calling predator is advertising territory, not hunting.
- **Movement audio.** Heavy footfalls, displaced vegetation, splashing. These sounds carry further than vocalizations during active pursuit.

### Prey Audio Design

Prey creatures serve as an ambient safety indicator:

- **High frequency, scattered.** Prey vocalizations sit in the 400Hz-2kHz range. Bright, active, distributed across the stereo field.
- **Density as safety metric.** A rich, active prey soundscape means the area is currently free of active predators. The player learns: lots of small creature sounds = safe to explore.
- **Alarm cascade.** When a predator enters an area, prey creatures go silent in a wave radiating outward from the predator's position. The player can track predator movement by listening to where the prey sounds cut out.

### Player Creature Audio

The player's own creature generates sound that changes with game state:

- **Breathing** -- Tempo and depth scale with exertion. Sprint breathing is loud and ragged. Resting breathing is barely audible. Critical health breathing is labored and irregular.
- **Footsteps** -- Surface-dependent (stone, vegetation, water, metal). Weight and impact change with evolution state. Early-game creature is light. Late-game evolved creature is heavier.
- **Vocalizations** -- The player creature makes involuntary sounds: pain grunts on damage, startled chirps from sudden threats, contented sounds in shelter. These are not player-controlled -- they're the creature reacting authentically.
- **Evolution changes** -- Post-evolution, the creature's entire audio signature shifts. Deeper breathing, heavier footsteps, new vocalization timbre. The player should feel physically different after evolving.

---

## 4. Weather and Environment Audio

### Rain System

Rain audio is stratified to match the visual rain system, creating a convincing three-dimensional rainfall:

- **Close drops (0-2 tiles)** -- Individual droplet impacts on nearby surfaces. Each drop is a distinct event. Surface material determines timbre: stone = sharp tap, vegetation = soft thud, water = plop, metal = ping.
- **Mid-distance rain (2-8 tiles)** -- Drops blur into a continuous wash. Still has some texture and variation. Intensity follows weather state.
- **Far rain shimmer (8+ tiles)** -- Pure atmospheric texture. A high-frequency wash that gives depth to the soundscape. Barely perceptible as individual drops.
- **Canopy interaction** -- Under tree canopy, close rain drops are replaced by larger, intermittent drops falling from leaves. The canopy acts as an audio filter, making rain sound softer and more irregular below it.
- **Interior rain** -- Inside structures, exterior rain is muffled. Specific leak points create localized drip patterns. Larger openings let more exterior rain sound through.

### Wind System

- **Directional modeling.** Wind audio pans across the stereo field to match wind direction. The player can feel which direction wind is blowing from audio alone.
- **Carried sounds.** Wind transports creature vocalizations and environmental sounds from adjacent zones. The player might hear a distant predator call carried on the wind before the creature is anywhere near visual range.
- **Surface interaction.** Wind through grass, through broken windows, around corners, across open water -- each creates a distinct timbre. The player reads terrain from wind sound.
- **Intensity scaling.** Calm breeze through gale-force gusts, with smooth transitions. High wind suppresses creature layer audio (creatures shelter in storms).

### Thunder

- **Irregular timing.** No rhythmic pattern. Thunder events are stochastic, following realistic delay-after-lightning timing.
- **Bass impact.** Thunder uses the lowest available frequencies. It should be physically startling even when expected.
- **Distance variation.** Close thunder: sharp crack followed by rolling bass. Distant thunder: pure low rumble with long decay.
- **No musical function.** Thunder is not used for dramatic punctuation. It happens because the weather system generated a storm, not because the game wants to create atmosphere.

### Water Audio

Water sounds are a primary navigation aid:

- **Flowing water** -- Indicates rivers, streams. Direction and volume tell the player which way water is moving and how large the flow is.
- **Dripping** -- Interior spaces, caves, ruins. Regular drip patterns indicate stable water sources. Irregular dripping indicates recent rain.
- **Standing water** -- Low-frequency lapping. The player's movement through standing water creates splashing that may alert nearby creatures.
- **Waves** -- Coastal zones. Rhythmic, bass-heavy. Wave timing is consistent within a weather state, giving the coastal zones a meditative quality that contrasts with inland biomes.

### Ruin-Specific Audio

Human structures have their own audio character that deepens as the player explores:

- **Structural sounds** -- Creaking metal, settling concrete, stressed cables. These are more frequent in recently-collapsed or unstable structures. Ancient, settled ruins are quieter.
- **Wind interaction** -- Broken windows whistle. Open corridors funnel wind. Enclosed rooms are sheltered.
- **Electrical remnants** -- In ruins with residual power (server farms, infrastructure), a persistent electrical hum. Transformers buzz. Capacitors whine. These sounds are alien in the natural world and immediately signal human-origin technology.
- **Echo and reverb** -- Interior spaces have appropriate reverb. Large open halls echo. Small rooms are dead. The player reads room size from audio before seeing it fully.

---

## 5. The Silence System

Silence is not the absence of design. It is the most important sound in the game.

### The Predator Warning

The primary use of silence is as a predator early-warning system:

1. **Normal state** -- Full ambient soundscape. Prey creatures vocalizing. Insects active. The world sounds alive.
2. **Alert cascade** -- A predator enters the area. Prey creatures nearest the predator go silent first. The silence spreads outward in a wave. Insects stop.
3. **Full silence** -- The predator is close. Only base atmosphere remains (wind, water). The absence of creature sound is deafening. The player's own breathing becomes prominent.
4. **Pursuit** -- If the predator engages, audio narrows to survival essentials. The player hears themselves and the predator. Everything else is stripped.

The player who learns to read the silence wave gains a critical survival advantage. They can detect predator approach, estimate distance, and even determine direction based on where the silence originates.

### Shelter Audio Contrast

Shelter moments use silence as relief:

- Outside: full weather, creature sounds, possible threats.
- Crossing shelter threshold: exterior sounds muffle. Interior quiet takes over.
- Inside: the player's breathing slows. Heartbeat (if at elevated health state) gradually calms. Rain on the shelter roof, if raining. The quiet is safety.

This contrast makes shelter genuinely feel like shelter. The audio transition is the primary communicator of "you are safe now."

### Pursuit Audio Stripping

During active predator pursuit, the mix undergoes aggressive subtractive mixing:

- Ambient layers pull down to 10-20% of normal.
- The player's breathing and footsteps become the dominant sound.
- The predator's movement audio is spatially precise and prominent.
- The player's heartbeat becomes faintly audible if health is low.
- Escape (breaking line of sight / reaching shelter) triggers a gradual restoration of the full mix. The ambient layers return over 5-10 seconds, signaling that the threat has passed.

---

## 6. Discovery Audio

### Ruin Exploration

Entering a ruin shifts the soundscape in stages:

1. **Threshold** -- Exterior ambience begins to muffle. Reverb characteristics change. The player feels the acoustic space shift.
2. **Interior** -- Ruin-specific sounds take over. Dripping, structural settling, wind through openings. The creature layer is mostly absent (few creatures inside ruins).
3. **Deep interior** -- If the ruin has depth, audio becomes increasingly isolated. The outside world is inaudible. The player is alone with the ruin's sounds.

### Narrative Fragment Discovery

The single exception to the no-music rule:

When the player discovers a narrative fragment -- a piece of the old world's story embedded in a ruin -- a **single sustained tone** emerges from the ambient mix. Not a melody. Not a chord. One tone. It resonates for 8-12 seconds, then fades.

This tone is:
- Pitched differently for each fragment (creating a very slow, scattered "melody" across the full game that the player may or may not consciously register).
- Slightly detuned, as if the memory itself is degraded.
- Spatially centered (not positioned in the stereo field like everything else), making it feel internal rather than external.
- The only non-diegetic sound in the game, which gives it enormous emotional weight through contrast.

### Server Farm Audio

The server farms are the game's most acoustically unique spaces:

- **The hum.** A complex, layered electrical drone. Multiple frequencies interacting. It is not pleasant. It is not unpleasant. It is deeply, fundamentally alien in the context of the natural world the player has been inhabiting.
- **Data noise.** Hard drives clicking. Cooling fans at varying speeds. Occasional digital artifacts -- not melodic, not rhythmic, just the sound of computation happening without purpose.
- **The uncanny quality.** These machines are still running. There is no one to run them for. The audio should communicate this existential wrongness. The machines sound busy. The world has moved on. The contrast between purposeful machine activity and purposeless existence is communicated primarily through audio.

---

## 7. Evolution Audio

### The Transformation

When the player creature evolves, the audio design must communicate biological transformation without either glorifying or horrifying it. It is a natural process -- unsettling in the way all biological processes are when observed closely, but not monstrous.

- **Onset** -- The creature's normal ambient sounds (breathing, movement) become irregular. A low-frequency biological vibration begins.
- **Transformation** -- Wet, organic sounds. Not gore -- closer to the sound of a plant growing rapidly, of joints popping, of skin stretching. Duration: 3-5 seconds.
- **Completion** -- A brief moment of near-silence as the new body settles. Then the creature's first breath in its new form. The breathing sounds different. Deeper, or faster, or with a new resonance. The player immediately hears that they are changed.

### Post-Evolution Audio Identity

After evolution, the creature's entire audio signature updates:

- **Breathing** -- New pattern, new timbre. Reflects the physical changes.
- **Footsteps** -- Different weight, different gait. If the creature gained a new limb type, new movement sounds appear.
- **Vocalizations** -- The creature's involuntary sounds shift register. Early creature: small, high, chirping. Late creature: deeper, more resonant, more complex.
- **New ability sounds** -- If evolution grants new abilities (climbing, swimming, gliding), those abilities have their own distinct audio. The first time the player uses a new ability, its sound should feel novel and earned.

---

## 8. Technical Implementation Notes

### Audio Engine Requirements

- **Polyphony budget:** 32 simultaneous voices maximum. Prioritized by proximity and gameplay relevance.
- **Spatial audio:** Full 2D stereo positioning with distance attenuation curves per sound category.
- **Occlusion:** Simplified ray-based occlusion for sounds passing through terrain/structures. Interior/exterior transitions use wet/dry mix adjustment.
- **Streaming:** Base atmosphere layers are streamed. One-shot creature/environment sounds are loaded per-biome.
- **Bus structure:**
  - Master
  - Atmosphere (base ambient layers)
  - Creatures (all creature vocalizations and movement)
  - Weather (rain, wind, thunder)
  - Environment (ruin sounds, water, structure)
  - Player (player creature sounds)
  - Discovery (narrative fragment tones)
- **Dynamic mixing:** Bus volumes are controlled by the game state manager. Transition curves are defined per mixing-state pair (exploration->tension, tension->pursuit, etc.) with configurable fade times.

### Asset Specifications

- **Format:** OGG Vorbis for streaming layers, WAV for one-shots.
- **Sample rate:** 44.1kHz.
- **Ambient loops:** Minimum 180 seconds, seamless loop points. At least 2 variation layers per biome atmosphere.
- **Creature sounds:** 3-8 variations per vocalization type per species, randomly selected with no-repeat logic.
- **Footsteps:** 4+ variations per surface type, round-robin playback.
